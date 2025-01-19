# Encoder Interpolator Board

*A Sorensen Heavy Industries production.*

## What Is It?

This project contains source files for hardware designed to interface with industry-standard 1vpp analog-output sin/cos encoders, and convert their output into a high-resolution interpolated digital quadrature signal. It uses iC-Haus' [iC-TW8](https://www.ichaus.de/product/ic-tw8/) 16-bit sin/cos interpolator IC in order to do this, and has an interface compatible with Renishaw's [REE analog interpolators](https://www.renishaw.com/resourcecentre/download/data-sheet-ree-analogue-interface--18359). However, unlike the REE series, it isn't limited to a fixed factory-set resolution and has a SPI interface.


![Interpolating some sines](output_example.png?raw=true)

## Revision History

### Revision 1

Although revision 0 is fully functional, hardware is never perfect. Revision 1 contains two minor changes:

  * The package and footprint of the MEMS oscillator used to generate an on-board 24MHz clock signal was changed from a large 7x5mm package
    (with an incorrect footprint prone to solder bridging) to a smaller 5x3.2mm package with a correct footprint.

  * The 5v power rail indicator LED was removed, as the 3.3v rail also has an indicator that implies a functional 5v rail. All of the remaining
    indicator LEDs' current-limiting resistors are now integrated into one 20K Ohm resistor network, simplifying assembly.
	 
## Configuration

The encoder interpolator boards may be configured in two ways: in hardware, via three jumpers and 8 configuration resistors, and in software, over a SPI
bus. The SPI command reference for this chip is only available via request from iC-Haus (in the "iC-TW8 16-BIT SIN/COS INTERPOLATOR Programmer’s Reference" document)
and will not be discussed further. Note that iC-Haus prefers totally-legitimate-seeming corporate email accounts. The full document ion for hardware-configurable
settings is available in the public datasheet, and a subset is reproduced here.

### Jumper Settings

Three solder jumpers are located on the back of the PCB.

  * **JP1** controls the drive voltage for the RS-422 ICs. In the default state it is closed and uses the system's 5V rail. If a different drive voltage
  is desired, this jumper may be cut and power supplied to the V422 pin header. If multiple boards are chained together and a different voltage is used,
  this jumper must be open on all of the boards.

  * **JP2** controls the use of an external clock signal to synchronize multiple boards, provided on the CLK pin header. In the default state, the system's internal clock is used. If
  the jumper is cut and soldered in the other position, the internal clock is ignored and the external clock signal is used. If the jumper is not cut, but
  all three pins are connected, the internal clock will be distributed over the CLK pin - it's unclear if this is a good idea, as the local clock oscillators
  don't have particularly high drive strengths. If the jumper is cut and not soldered in the other position, nothing will work. Don't do that.

  * **JP3** controls the configuration mode of the iC-TW8. In its default state, the IC's configuration is taken from the 8 configuration resistors. If
  this jumper is cut, the IC must be configured over SPI.

### Resistor Settings

Resistors R10-R17 are arranged to form four resistor dividers, generating configuration voltages that control the iC-TW8's interpolation factor and signal
processing modes.

|Configuration Value | R_Top | R_Bottom | Function(s)          |
|:--------------------:|:-------:|:----------:|----------------------|
| C0                   | R10     | R11        | Interpolation factor |
| C1                   | R12     | R13        | Interpolation group, Auto-adaption Mode |
| C2                   | R14     | R15        | Hysteresis, Filtering Mode|
| C3                   | R16     | R17        | Output Frequency Limit,  Lag Recovery |


![Top/Bottom Resistor Location](fabrication_exports/Conf_Resistors.png?raw=true)


Each resistor divider may output one of twelve voltage levels, set with resistor values recommended by iC-Haus.

| Level | Ratio | R_Top (kΩ) | R_Bottom (kΩ) |
|-------|-------|:------------:|:---------------:|
|11 | 1.00 | 0.00 | ∞   |
|10 | 0.80 | 3.01 | 12.1|
|9  | 0.73 | 4.02 | 11.0|
|8  | 0.67 | 4.99 | 10.0|
|7  | 0.60 | 6.04 | 9.09|
|6  | 0.54 | 6.98 | 8.06|
|5  | 0.46 | 8.06 | 6.98|
|4  | 0.40 | 9.09 | 6.04|
|3  | 0.33 | 10.0 | 4.99|
|2  | 0.27 | 11.0 | 4.02|
|1  | 0.20 | 12.1 | 3.01|
|0  | 0.00 | ∞    | 0.00|


Depending on the interpolation group value of C1, a fixed value for C0 results in different interpolation values:

| C0 Voltage Level | Interpolation Group 0 | Interpolation Group 1 | Interpolation Group 2 |
| :---: | :---: | :---: | :---: |
|11| x8192 | x1600 | *x10000* |
|10| x4096 | x800  | *x5000*  |
|9 | x2048 | x400  | x2500    |
|8 | x1024 | x200  | x1250    |
|7 | x512  | x160  | x1000    |
|6 | x256  | x100  | x500     |
|5 | x128  | x80   | x250     |
|4 | x64   | x50   | x125     |
|3 | x32   | x40   | x62.5    |
|2 | x16   | x20   | x25      |
|1 | x8    | x10   | *x12.5*  |
|0 | x4    | x5    | *x6.25*  |

Values in *italics* have limitations detailed in the full datasheet.

C1 controls both the interpolation group in the previous table and how much clever signal processing the IC is performing - its auto-adaption mode. The following table gives the required voltage level for C1 as a function of desired interpolation group (row) and adaption mode (column).

|Interpolation Group | A3/Full analog/digital | A2 / Full digital (Recommended) | A1/Partial digital | A0 / None |
|:---:| :---:| :---:|:---:|:---:|
| **Group 0** | 9 | 6 | 3 | 0 |
| **Group 1** | 10 | 7 | 4 | 1 |
| **Group 2** | 11 | 8 | 5 | 2 |


C2 controls hysteresis and filtering of the AB outputs - again, the following table gives the required voltage level as a function of the desired hysteresis (row) and filtering (column).

|Hysteresis (AB Edge Count) | Heavy Filtering | Normal Filtering (Recommended) | Minimal Filtering |
|:---:| :---:| :---:|:---:|
|**1**| 11 | 7 | 3 |
|**2**| 10 | 6 | 2 |
|**4**| 9  | 5 | 1 |
|**8**| 8  | 4 | 0 |

C3 controls AB output frequency (row) and lag recovery (column).

| Maximum AB Frequency | Maximum AB Frequency (24MHz clock) | Lag Recovery Enabled | Lag Recovery Disabled (Recommended) |
|:---:| :---:| :---:|:---:|
|*f<sub>clock</sub>*/8 | 3.0 MHz | 11 | 5 |
|*f<sub>clock</sub>*/16 | 1.0 MHz | 10 | 4 |
|*f<sub>clock</sub>*/32 | 0.75 MHz | 9 | 3 |
|*f<sub>clock</sub>*/64 | 0.375 MHz | 8 | 2 |
|*f<sub>clock</sub>*/128 | 187.5 KHz | 7 | 1 |
|*f<sub>clock</sub>*/256 | 93.75 KHz | 6 | 0 |


## Future Directions

As this board was designed for a specific application that revision 0 satisfies, it's unlikely to see significant further changes. However, the choice of 3.3v as a system
voltage leaves quite a bit of performance on the table by limiting the clock speed to 24MHz, instead of 32MHz. This choice was made for comparability with the REE pinouts,
which only have a 5V supply - ideally, the noisy RSS-422 drivers would live on a separate voltage rail than the main IC voltage supply.

A library for SPI communication with the IC is a more likely future outcome.


# Encoder Interpolator Board

*A Sorensen Heavy Industries production.*

## What Is It?

This project contains source files for hardware designed to interface with industry-standard 1vpp analog-output sin/cos encoders, and convert their output into a high-resolution interpolated digital quadrature signal. It uses iC-Haus' [iC-TW8](https://www.ichaus.de/product/ic-tw8/) 16-bit sin/cos interpolator IC in order to do this, and has an interface compatible with Renishaw's [REE analog interpolators](https://www.renishaw.com/resourcecentre/download/data-sheet-ree-analogue-interface--18359). However, unlike the REE series, it isn't limited to a fixed factory-set resolution and has a SPI interface.


![Interpolating some sines](output_example.png?raw=true)

## Revision History

### Revision 2

Cyrus Lloyd kindly pointed out that the rev 0/1 output connector pinout actually bears absolutely no resemblance to the REE family interface - a minor oversight with no functional consequences, clearly, and not a failure of the entire raison d'être of the project. Revision 2 fixes this, but is as of yet untested.

### Revision 1

Although revision 0 is fully functional, hardware is never perfect (see revision 2). Revision 1 contains two minor changes:

  * The package and footprint of the MEMS oscillator used to generate an on-board 24MHz clock signal was changed from a large 7x5mm package
    (with an incorrect footprint prone to solder bridging) to a smaller 5x3.2mm package with a correct footprint.

  * The 5v power rail indicator LED was removed, as the 3.3v rail also has an indicator that implies a functional 5v rail. All of the remaining
    indicator LEDs' current-limiting resistors are now integrated into one 20K Ohm resistor network, simplifying assembly.


## Fabrication


This cost ($18) and limited availability of the main IC on this board essentially rules out simply ordering finished boards from the usual low-cost PCBA vendors, so  hand assembly is the best option for populating this board. I ordered r0 boards from PCBWAY in their usual-spec 4 layer process (HASL, 1oz Cu), along with a stainless steel solder stencil, placed all of the front-side components by hand, and reflowed the board on a 30x30mm Miniware hotplate. Despite getting very clean solder paste prints and otherwise perfect reflow, I consistently got solder bridges on the QFN-48 package - although easily fixed with an iron, some combination of Kicad's footprints, my stencil thickness, and my solder paste resulted in a bit too much solder.

### Bom

|Reference                                  |Qty|Value                 |Footprint    |Manufacturer      |MPN                 |Mouser PN               |Notes                                    |
|:----------------------------------------- |:-:|:-------------------- |:-----------:|:----------------:|:------------------:|:----------------------:|-----------------------------------------|
|                C1, C2, C3                 | 3 |   47pF 10V C0G 5%    |0603 imperial|      Wurth       |    885012006006    |    710-885012006006    |                                         |
|          C4, C5, C6, C7, C8, C9           | 6 |   470pF 10V C0G 5%   |0603 imperial|      Wurth       |    885012006012    |    710-885012006012    |                                         |
|                 C10, C17                  | 2 |   10uF 25V X5R 10%   |0805 imperial|     Samsung      |  CL21A106KAYNFNE   |  187-CL21A106KAYNFNE   |                                         |
|C11, C12, C13, C14, C15, C16, C18, C20, C22| 9 |   0.1uF 25V X7R 5%   |0603 imperial|   Taiyo Yuden    |  EMK107B7104KA-T   |  963-EMK107B7104KA-T   |                                         |
|                    C19                    | 1 |   22uF 25V X5R 10%   |1206 imperial|     Samsung      |  CL31A226KAHNNNF   |  187-CL31A226KAHNNNF   |                                         |
|                    C21                    | 1 |   1uF 16V X7R 10%    |0603 imperial|       TDK        |CGA3E1X7R1C105K080AC|810-CGA3E1X7R1C105K080AC|                                         |
|                    D1                     | 1 |     Red 0603 LED     |0603 imperial|      Wurth       |   150060RS75000    |   710-150060RS75000    |                                         |
|                  D2, D3                   | 2 |    Blue 0603 LED     |0603 imperial|      Wurth       |   150060BS75000    |   710-150060BS75000    |                                         |
|                    D5                     | 1 |    Green 0603 LED    |0603 imperial|      Wurth       |   150060GS75000    |   710-150060GS75000    |                                         |
|                    FB1                    | 1 |Ferrite Bead 60Ohm 25%|0805 imperial|Murata Electronics|   BLM21PG600SN1D   |     81-BLM21P600S      |                                         |
|                    J1                     | 1 | DA15 Male Horizontal |             |      Wurth       |    618015231421    |    710-618015231421    |                                         |
|                    J2                     | 1 |DA15 Female Horizontal|             |      Wurth       |    618015231121    |    710-618015231121    |                                         |
|                R1, R2, R3                 | 3 |         120R         |0603 imperial|    Panasonic     |    ERJ-S03J121V    |    667-ERJ-S03J121V    |                                         |
|        R4, R5, R6, R7, R8, R9, R19        | 7 |         R1K          |0603 imperial|      Yageo       |   RC0603JR-071KL   |   603-RC0603JR-071KL   |                                         |
|  R10, R11, R12, R13, R14, R15, R16, R17   | 6 |       various        |0805 imperial|                  |                    |                        |See "Configuration" section for values   |
|                    R20                    | 1 |         R20K         |0603 imperial|      Yageo       |  RC0603JR-0720KL   |  603-RC0603JR-0720KL   |                                         |
|               RN1, RN2, RN3               | 3 |         R10K         |  quad 0603  |      Yageo       |  YC164-JR-1010KL   |  603-YC164-JR-1010KL   |                                         |
|                    RN4                    | 1 |         R20K         |  quad 0603  |      Yageo       |  YC164-JR-0720KL   |  603-YC164-JR-0720KL   |                                         |
|                    SW1                    | 1 |SMT push button switch|             |       C&K        |  KMR232NG ULC LFS  |   611-KMR232NGULCLFS   |                                         |
|                  U1, U2                   | 2 |       UA9638C        |SOIC-8 1.27mm|Texas Instruments |     UA9638CDR      |     595-UA9638CDR      |                                         |
|                    U3                     | 1 |        ICTW8         |QFN-48 0.5mm |     iC-Haus      |       ICTW8        |                        |Puchased in Q5 through the US distributor|
|                    U5                     | 1 |      24AA02-OT       |  SOT-23-5   |    Microchip     |     24AA02-OT      |   579-24AA02E48TIOT    |                                         |
|                    U6                     | 1 |   MCP1754S-3302xMB   |  SOT-89-3   |    Microchip     |  MCP1754ST3302EMB  |  579-MCP1754ST3302EMB  |                                         |
|                    X1                     | 1 | DSC1001BE5-024.0000  |   5x3.2mm   |    Microchip     |DSC1001BE5-024.0000 |   579-001BE50240000    |                                         |


	 
## Configuration

The encoder interpolator boards may be configured in two ways: in hardware, via three jumpers and 8 configuration resistors, and in software, over a SPI
bus. The SPI command reference for this chip is only available via request from iC-Haus (in the "iC-TW8 16-BIT SIN/COS INTERPOLATOR Programmer’s Reference" document)
and will not be discussed further. Note that iC-Haus prefers totally-legitimate-seeming corporate email accounts. The full documentation for hardware-configurable
settings is available in the public datasheet, and a subset is reproduced here.

After changing any settings or a new encoder, running the auto-calibration routine is recommended, and may be accomplished by holding down the push button, moving the encoder through a number of cycles, and releasing the button.

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
|11| x8192 | x1600 |**x10000**|
|10| x4096 | x800  |**x5000** |
|9 | x2048 | x400  | x2500    |
|8 | x1024 | x200  | x1250    |
|7 | x512  | x160  | x1000    |
|6 | x256  | x100  | x500     |
|5 | x128  | x80   | x250     |
|4 | x64   | x50   | x125     |
|3 | x32   | x40   | x62.5    |
|2 | x16   | x20   | x25      |
|1 | x8    | x10   |**x12.5** |
|0 | x4    | x5    |**x6.25** |

Values in **bold** have limitations detailed in the full datasheet.

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


# 32LC
32LC Calculator Project

Beta version 0.92 notes:

Added RCL+, RCL-, RCL*, RCL/
Added INPUT command
Added flags - program will automatically halt on overflow.  flag 5 has no effect, since it is not implemented yet.

Beta version 0.9 notes

Advantage of C implementation is speed, but not using calc. ROM means some aspects of the calculator may seem different than original.  

Features:

- 26 registers and 1,400 program steps
- 2 selectable display fonts
- 3 selectable display colors
- audible keyboard beep
- fast - approx. 77,000 loop iterations in 10 seconds
- internally has number range from approx. 1e-317  to 9.9e307
- solver using secant method
- integration using Simpson’s method
- complex number functions

Known issues / differences from 32S:

- EEX implemented slightly differently - single digit exponent processed immediately
- Eng notation and All display mode not implemented yet
- XEQ currently works like GTO
- INPUT, SHOW, VIEW and MEM not implemented yet
- COMPLEX and statistics functions are not programmable
- Indirect register addressing and GTO not implemented yet
- SF, CF and FS? not implemented yet
- BASE menu not implemented yet
- for Solver and integration, known variables must be manually entered beforehand
- in programming mode, each number digit requires a single step
- programming mode does not insert lines - currently does overwrite
- error messages need updating

Program memory organization

The 1,400 program steps are organized as follows:

0-99 general
100-149 LBL A
150-199 LBL B
200-249 LBL C
…
1350-1399 LBL Z

If LBL A is over 50 steps, it can overlap over B, and so on.



Solver example - Pythagorean formula:

Since a^2 + b^2 = c^2, then a^2 + b^2 - c^2=0

A00 LBL A
A01 RCL A
A02 x^2
A03 RCL B
A04 x^2
A05 +
A06 RCL C
A07 x^2
A08 -
A09 R/S

Exit programming mode.

Solve for C:

3 STO A
4 STO B

Enter 8 and 0 on the stack as initial guesses.

Set FN to (LBL) A
Solve for C

Result is 5

Solve for A:

4 STO B
5 STO C

0 STO A

Enter 8 and 0 on the stack as initial guesses.

Set FN to (LBL) A
Solve for A

Result is 3

Solve for B:

3 STO A
5 STO C

0 STO B

Enter 8 and 0 on the stack as initial guesses.

Set FN to (LBL) A
Solve for B

Result is 4



Integration:

B00 LBL B
B01 RCL X
B02 x^2
B03 R/S

Set Fix 4
Set FN to (LBL) B
Put lower limit 0 and upper limit 10 on the stack.
Integrate X.

Result is 333.3333



Monte Carlo program to calculate PI:

C00 LBL C
C01 RANDOM
C02 x^2
C03 RANDOM
C04 x^2
C05 +
C06 SQRT
C07 1
C08 x>y
C09 GTO D
C10 1
C11 STO+ A
C12 GTO C

To enter program D, use GTO . D or LBL D

D00 LBL D
D01 1
D02 STO+ A
D03 1
D04 STO+ B
D05 GTO C

Do XEQ C and let it run for about 10 seconds.  A stores the number of loop cycles and B stores the number of times a random point falls within quadrant 1 of a circle.

Do Rcl B, then Rcl A,  / (divide) and 4 x (times) to get an approximation of pi.


Burning firmware:

This 32LC firmware may be used for non-commercial personal or educational purposes.  No warranties are made on the use of the firmware.  It is up to the user to verify the accuracy or precision of this calculator implementation.

Go to Github riker2072 32LC repository.

Get 32LC.ino.bin, 32LC.ino.bootloader.bin and 32LC.ino.partitions.bin files.

Get Windows ESP32 flash download tool at: https://www.espressif.com/en/support/download/other-tools

Click on … to select 32LC.ino.bin file directory location.  In the @ box, put 0x10000
Click on … to select 32LC.ino.bootloader.bin file directory location.  In the @ box, put 0x0000
Click on … to select 32LC.ino.partitions.bin file directory location.  In the @ box, put 0x8000

SPI speed is 40MHz, SPI mode is DIO.  Check mark in the box labeled “DoNotChgBin”.  My port settings are COM4, baud 115200.  Connect the M5 Cardputer to your PC using a USB C cable.  Click on START to burn the firmware.

File for black and white overlays are also on Github.



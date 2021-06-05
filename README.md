# MSP432P401R

**IN PROCESS...**

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND (see LICENSE)

## Hardware:
- MSP432P401R      ( Mouser No: 595-MSP-EXP432P401R  )
- HD44780 16x2 LCD ( Mouser No: 992-LCD-16X2B        )
- 2x 10kOhms Potentiometers, breadboard, wires.

## Instructions:
1. **Setup/Verify**
	- Connect MSP432 via USB
	- Create "New CCS Project" in Code Composer Studio
		- select Target: MSP432 Family  |  MSP432P401R
		- click "Verify" to make sure the Lanuchpad works
		- name the project
		- leave the rest of settings untouched
2. **Add LCD lib:**
	- Add both .c and .h files of the [LCD16x02_MSP43x library](https://github.com/agaelema/LCD16x2_MSP43x) by [agaelema](https://github.com/agaelema) via "Project Explorer" window into the project's main directory
	- Edit the .h lib file to fit MSP432 PIN Layout instead of MSP430
		- Line 22: #include <msp432.h>
		- Line 28: #define     _EN_PORT        4                   // default EN port - P4
		- Line 29: #define     _EN_PIN         2                   // default EN pin  - P4.2
		- Line 30: #define     _RS_PORT        4                   // default RS port - P4
		- Line 31: #define     _RS_PIN         0                   // default RS pin  - P4.0
		- Line 33: #define     _LCD_4BIT_PORT          4               // port used to data
		- Line 34: #define     _LCD_DATA_BASE          4               // first low pin of 4 bit interface

3. **main.c**

		#include <msp432.h>
		#include <stdint.h>
		#include "lcd16x2_msp43x.h"

		void main(void)
		{
		    P4->DIR = 0xFF;     // Set P4 pins as output
		    P4->OUT = 0x00;     // Clear all pins

		    lcd16x2_Init();                       // Initialize LCD

		    lcd16x2_SetPosition(0,0);             // Set position of the cursor (row, column)
		    lcd16x2_PrintString("MSP432P401R");   // Print text

		    lcd16x2_SetPosition(1,0);
		    lcd16x2_PrintString("16x2 LCD");


		    /* The underscore at the end of printed text isn't any kind of error,
		       it's just a cursor and it's possible to manipulate its state
		       in lcd16x2_msp43x.c or directly from main.c */

		    //lcd16x2_Cmd(LCD_CMD_DISPLAY_ON_CURSOR_OFF);     // display ON, cursor OFF/ON/BLINK
		                                                    // lines 101-103 in lcd16x2_msp43x.h
		}


4. **Connect the hardware:**
	
### !!! WARNING: Disconnect the Launchpad from PC first !!!

- In case there're slightly different pin names on LCD module, see "INFO/HELP" section in the end.

- R1 250Ohm (or 200-300 Ohms) is placed just in case not to damage the backlight, cutting a little from the highest current.

![Wiring scheme](/images/WIRING.BMP)

- Check the documentation for your particular potentiometers to find the right connection depending on its type.

- For a linear slide POT i have ( Mouser No: 652-PTA20432015CPB10 ) it seems a bit dfferent and counterintuitive in some way than for a single turn POT which you can see on demo gifs:

![Slide POT (potentiometer) wiring](/images/SLIDE_POTENTIOMETER.BMP)

![Single turn POT wiring](/images/SINGLE_TURN_POT.BMP)


5. **Build (Project->Build Project)**

6. **Connect Launchpad to PC and load (Run->Load->Select Program To Load)**

7. **(Optional) Open repo's archived CCS project:**
	- Unarchive the project
	- Project->Import CCS Project->Select search-directory
	- Project->Clean
	- Project->Build
	- Run->Load->Select Program To Load


___

## DEMONSTRATION:

CONTRAST_DEMO

![CONTRAST_DEMO](/images/CONTRAST_DEMO.gif)


BRIGHTNESS_DEMO

![BRIGHTNESS_DEMO](/images/BRIGHTNESS_DEMO.gif)

___

<details>
	<summary>INFO/HELP</summary>

- In case the lib link is broken or you cannot find it for some other reason, the whole repo "LCD16x2_MSP43x-master.zip" included "AS IS" in "/archive".

- LCD PIN Description:

Pin number | Pin name  | Pin description
--- | --- | ---
1 | GND/VSS | ground pin for LCD
2 | VDD/VCC | supply voltage pin for LCD
3 | V0/VEE | contrast adjust
4 | RS | Register Select (0;1)
5 | RW | Read/Write (0;1)
6 | E | enable
7-14 | Data Bits | send commands or data to the LCD
15 | A/LED+ | supply voltage pin for backlight
16 | K/LED- | ground ping for backlight

</details>

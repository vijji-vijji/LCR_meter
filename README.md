Overview
--------------------
* Name : LCR_meter
* Title : Inductance, Capacitance and Resistance meter, Arduino NANO based.
* Description : 
	* Inductance, Capacitance and Resistance meter.
	* Arduino NANO version 3.0 based. 
	* 4 resistor test ranges,  Resistor range is from 0 ohms to 2 Mohms. Auto-range.
	* Inductance test, range is 80uH to 30,000uH.
	* Capacitance three tests , range 18pf to 4F. 
	* Push button input.
	* Outputs data to serial monitor and a HD44780 LCD 16x02.

* Platform: Arduino NANO version 3.0
* Author: Gavin Lyons

Table of contents
---------------------------

  * [Overview](#overview)
  * [Table of contents](#table-of-contents)
  * [Software and Libraries](#software-and-libraries)
  * [Parts List](#parts-list)
  * [Project Schematic](#project-schematic)
  * [Control](#control)
  * [Example Output](#example-output)
  * [C Meter](#c-meter)
  * [R Meter](#r-meter)
  * [L Meter](#l-meter)
  * [Copyright](#copyright)

Software and Libraries
------------------
Arduino 1.8.9

Eagle 9.5.2

* Only library used for LCD = LiquidCrystal_I2C.h //V.1.1.2 [link](https://github.com/marcoschwartz/LiquidCrystal_I2C)

Parts List
------------------------------
The schematic and BOM is available in documents folder.

>
>  HD44780-based LCD 16x02 with I2C PCF8574 interface module 
>
> Arduino NANO  micro-controller module Version 3.0
>
> Resistors: 2kohms, 20kohms, 200kohms, 1Mohms, 150ohm, 330ohm, 4.7Kohms X 2 ,
> Cap Test2(10Kohm and 220ohm) , Cap Test3(10K, 3.1K, 1.8K)
>
> Capacitors: 2 uF non-polarized  
>
> Two push buttons.
>
> Diode: 1N4001 or IN4148
>
> LM393 comparator IC [datasheet](https://www.onsemi.com/pub/Collateral/LM393-D.PDF) 
>
> ZIF test socket 16 pin. 
>

Project Schematic 
----------------------------

![ScreenShot project sch](https://github.com/gavinlyonsrepo/LCR_meter/blob/master/documentation/eagle/LCR_meter.png)

Pinout Connections Tables.

| Arduino Nano | Desc                               | SW Name          |
|--------------|------------------------------------|------------------|
| D13          | capacitor test 1 charge pin        | Cap1chargePin    |
| D12          | PUSH button Mode                   | buttonMode       |
| D11          | Push button TEST                   | buttonTest       |
| D10          | resistor apply Vin                 | ApplyResVoltage  |
| D9           | resistor range  Upper              | Res1M            |
| D8           | resistor range  Upper MID          | Res200K          |
| D7           | Cap test 3 volt ref                | AIN1         |
| D6           | Cap test 3 connector               | AIN0         |
| D5           | Inductor out                       | OutLtestPin      |
| D4           | inductor in                        | PulseInPinLtest  |
| D3           | resistor range   lower MID         | Res20K           |
| D2           | resistor range   Lower             | Res2K            |
| A0           | capacitor test 1 discharge pin     | Cap1dischargePin |
| A1           | Cap test 3  pulse pin              | Cap3pulsePin     |
| A2           | capacitor test 2 negative  ADC     | Cap2InPin        |
| A3           | capacitor test 2 positive  ADC     | Cap2OutPin       |
| A4           | LCD SDA I2C                        | DATA SDA     |
| A5           | LCD CLK I2C                        | CLK  SCLK    |
| A6           | capacitor test 1 ADC               | Cap1analogPin    |
| A7           | Resistor Test  ADC                 | analogResPin     |


| Test Socket PCB pin | Name                        | NANO Pin |
|---------------------|-----------------------------|----------|
| 1                   | resistor test Voltage apply | D10      |
| 2                   | n/a                         | n/a      |
| 3                   | ground                      | ground   |
| 4                   | ground                      | ground   |
| 5                   | n/a                         | n/a      |
| 6                   | n/a                         | n/a      |
| 7                   | resistor test Analog in     | A7       |
| 8                   | """                         | """      |
| 9                   | Inductor Circuit            | n/a      |
| 10                  | C2 test Postive             | A3       |
| 11                  | n/a                         | n/a      |
| 12                  | C2 test negative            | A2       |
| 13                  | ground                      | ground   |
| 14                  | ground                      | ground   |
| 15                  | C3 test                     | D6       |
| 16                  | C1 test                     | A6       |


Control
-------------------------------

Push Buttons
1. Mode Button to change mode.
2. Test button to start test for selected mode, when in ready mode this button brings up help screen.

Tests Modes:

1. Inductance test.
2. 0 to 2k range resistor test.
3. 2K to 20k range resistor test.
4. 20k to 200k range resistor test.
5. 200k to 1M range resistor test.
6. Capacitor Test 1 Range  1 uF to 4F.
7. Capacitor Test 2 Range 18 pF to 470 uF.
8. Capacitor Test 3 Range 4.7 nF to 180 uF.

The meter outputs to an LCD and serial monitor.
(The LCD is optional as data is sent via serial monitor to PC)
Every point in code that requires User calibration on Initial build is marked "USER CALIBRATE"

Example Output
-----------------------------

Example output for Capacitor test 2 on a 10uF capacitor to LCD and serial monitor.

![cap lcd1](https://github.com/gavinlyonsrepo/LCR_meter/blob/master/documentation/images/LCRmeterlcd1.jpg)

![cap lcd2](https://github.com/gavinlyonsrepo/LCR_meter/blob/master/documentation/images/LCD.jpg)

![ScreenShot cap serail](https://github.com/gavinlyonsrepo/LCR_meter/blob/master/documentation/images/LCRmeterserial.jpg)


C Meter
-----------------------------------------------

Each Arduino capacitance test relies on a property of resistor capacitor (RC) circuits- the time constant. 
The time constant of an RC circuit is defined as the time it takes for the voltage across the capacitor 
to reach 63.2% of its voltage when fully charged:

![ScreenShot cap time constant](https://github.com/gavinlyonsrepo/Cap_meter_arduino/blob/master/images/ctc.jpg)

Larger capacitors take longer to charge, and therefore will create larger time constants. 
The capacitance in an RC circuit is related to the time constant by the equation:

![ScreenShot cap eq constant](https://github.com/gavinlyonsrepo/Cap_meter_arduino/blob/master/images/eq.jpg)

The capacitance meter has an RC circuit with known resistor values and an unknown capacitor value. 
The Arduino will measure the voltage at the capacitor and record the time it takes to reach 63.2% of it’s voltage when fully charged (the time constant). 
Since the resistance value is already known, 
we can use the formula above in a program that will calculate the unknown capacitance.

**Test 1**

Test 1: Range  1 uF to 4F. 
Uses two digital pins, one analog pin, and two resistors.
One for discharge, one for charge.
Insert a capacitor in range into terminal press button and view result on LCD or serial monitor.
The Capacitor is charged,  We can measure elapsed time until time constant fulfilled using analog pin and we know R 
value. Therefore we solve for C. 
The result gives two values: Capacitance value and time constant to test in mS.
The user can adjust the 10K resistor value in code variable(resistorValue) to match exact value, to improve accuracy.

![ScreenShot cap test1](https://github.com/gavinlyonsrepo/LCR_meter/blob/master/documentation/images/cap_test1.png)

**Test 2**

Test 2 : Range 18 pF to 470 uF.

This capacitance test has the greatest range of 3. It also had the highest accuracy with smaller capacitors. 
No resistors are needed and it only uses two analog pins(A2(neg) and A3(positive) from the Arduino.
There is no extra resistor just the internal components of ATmega328 chip.

C1 calibration(optional): 

The internal analog pin capacitor to gnd (C1) varies between 20 and 30pF. unknown capacitor = Cu.
The positive pin is set to 5V, the negative to 0V.
VA2 = (VA3 * Cu)/(C1 + Cu).
The value of C1 for any given Arduino board will have to be calibrated using a known cap for Cu,
for full accuracy I set it at 24.48pF in code (CapOne) Found using equation.
C1 = Cu * (VA3-VA2) / VA3, Where  Va2 is ADC value reported by code and VA3 is 5v or 1023.
So for a known capacitor of 103pF giving a ADC value of 801.
So C1 = 103pf * (1023-801)/ 1023 = 28.48pF.
The user should change this for their system to improve accuracy.
Test a known cap get ADC value and then pop these values into equation above
and put the C1 value into (CapOne) variable in code.

Test equation:

Cu = VA2 * C1 / (VA3 - VA2)

The test displays 3 values 
* Cu,  Capacitance in Farads 
* Time constant for test in mS
* Va2 , ADC measurement from 0 to 1023 where 1023 is 5V 

![ScreenShot cap test2](https://github.com/gavinlyonsrepo/LCR_meter/blob/master/documentation/images/cap_test2.png)

**Test 3** 


Test 3 Range 0.0047 uF to 180 uF.

![ScreenShot cap test3](https://github.com/gavinlyonsrepo/LCR_meter/blob/master/documentation/images/cap3_test3.png)

A voltage divider is created to give a reference voltage,

Vout = Vin * (R5 / (R5 + R4)) , 5 * (3100 / (1800 + 3100)) = 3.163.

The objective here is to find the time constant τ (tau) in the equation:

τ = R * C Solved for C: C = τ / R

So we want to charge the capacitor under test with a suitable voltage (eg. 5V from an Arduino output pin) and measure how long it takes to reach 63.2% of that voltage. So we need a reference voltage of 5 * 0.632, namely 3.16V.
The internal analogue comparator is used for this job.:

1. pin 6 PD6 AIN0
2. pin 7 PD7 AIN1
 
We connect the reference voltage to the AIN1 pin (negative reference) and connect our capacitor to the AIN0 pin (positive reference) and then configure an interrupt on the rising edge. A value of 10K for the resistor to give a reasonably slow charge time. The value of ~ 10K is specified in code variable (resistance).
so adjust for your resistor value. Make sure to pick accurate resistors for voltage divider.

The test returns the Capacitance result and time constant.

R Meter
------------------------------------

The resistors used in circuit should be measured on good quality meter 
and there exact values added to code for maximum accuracy. 
This Resistance test can auto-range in both directions. 

0-2kohms : 2kohms resistor

2k-20k : 20k resistor

20k-200k : 200k resistor

200k - 1Mohm : 1 Mohm resistor

So we will use a basic voltage divider to calculate the resistance. 

![ScreenShot resistor](https://github.com/gavinlyonsrepo/LCR_meter/blob/master/documentation/images/R_TEST.png)

A voltage divider is made of two Resistances (R1 and R2) in series. The output voltage in the middle point is [R2/(R1+R2)]Vin. Using this formula and knowing the value of R2 and measuring the Vout calculate the resistance of R1. 
Vin will be 5 volts from the Arduino, We can measure Vout with one of the Arduino's analog inputs.
We need to use different ranges of R2 to measure with better precision. 

L Meter
-----------------------------------------------

This tests range is from 80uH to 30,000uH. Ideally the capacitors in circuit must be in specification and measured on a good quality meter. The unknown test inductor is placed in parallel with a capacitor thus creating an LC circuit. 

As soon as the voltage on the LC circuit becomes positive, the LM393 will be floating, which can be pulled high with a pull up resistor. When the voltage on the LC circuit becomes negative, the LM393 will pull its output to ground. 

![ScreenShot l](https://github.com/gavinlyonsrepo/LCR_meter/blob/master/documentation/images/L_TEST.png)

A 5V pulse signal from Arduino is applied to the LC circuit. We charge the circuit for some time. Then we change the voltage from 5 volts directly to 0. That pulse will make the circuit resonate creating a  sinusoidal signal oscillating at the resonant frequency. The frequency is measured and later using the formulas we can obtain the inductance value. We will use the Arduino to measure the frequency and calculate the value. The resonant frequency is measured with the next equation: 

![ScreenShot seq 1](https://github.com/gavinlyonsrepo/LR_meter_arduino/blob/master/documentation/images/eq1.png)

So we could obtain the L value because we know the F frequency that we've just measured and we also know the value of the capacitor because it's a component that we've selected. All we need is to obtain L from this equation.


![ScreenShot seq 2](https://github.com/gavinlyonsrepo/LR_meter_arduino/blob/master/documentation/images/eq2.png)

The comparator will turn the sine wave into a square wave with a duty of 50%(see fig 9 in 
[datasheet](https://www.onsemi.com/pub/Collateral/LM393-D.PDF)),  The microcontroller measures the signal , This measurement can then be doubled to get the period and the inverse of the period is the frequency. Since the circuit is resonating, this frequency is the resonating frequency.

We can now solve the equation for inductance.


Copyright
-------------------------------

Copyright (C) 2019 G Lyons. This program is free software; you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, see license.md for more details

# Blackpill STM32F401CE I2C+UART+SSD1306+TMP102 Example

*December 2020*

This example will create a temperature monitor using the SparkFun TMP102 breakout board.  Temperature data will be printed to the UART and displayed on the SSD1306 OLED.
Both the TMP102 and the SSD1306 are using the I2C communication protocol.

It uses the ST toolchain which consists of two applications:

* STM32CubeMX  (Version 6.1.0)
* STM32CubeIDE (Version 1.5.0)

Both can be downloaded from ST: https://www.st.com/content/st_com/en/stm32cube-ecosystem.html

STM32CubeMX is a configuration tool for setting up a software project with the right pin, peripheral, and software libary configuration. It supports multiple IDE options including STM32CubeIDE.

STM32CubeIDE is an Integrated Development Environment which allows a programmer to edit, compile, and debug from the same tool.  It is based on the very popular Eclipse IDE and bundles gcc for ARM.

# Credits

The Shawn Hymel has an excellent STM32 series on YouTube.  "Getting Started With STM32 and Nucleo Part 2: How to Use I2C to Read Temperature Sensor TMP102" was very helpful.

https://www.youtube.com/watch?v=isOekyygpR8&t=14s

There are several libraries for the SSD1306 but my source was ControllersTech:

https://controllerstech.com/oled-display-using-i2c-stm32/

The library code itself cites:

   	Copyright (C) Alexander Lutsai, 2016
    Copyright (C) Tilen Majerle, 2015
    

# STM32CubeMX

The first step in creating a new project is to run STM32CubeMX and configure the I/O and peripherals.

## New Project

This is the default starting place and it gives you three options:

- Create a project based on an MCU.
- Create a project based on an ST Development Board.
- Create a proejct based on an example.

For the Blackpill, you will need to choose the first option, MCU.

### MCU Selector

Type in your MCU.  Mine was the STM32F401CE.

Notice that there is some good information on this page!  

- Features
- Block Diagram
- Docs & Resources
- Datasheet

When you're done with the research, click the **[ Start Project ]** button.

## Pinout & Configuration

Note the *Categories* window on the left.


## System Core

### RCC

> Set the **High Speed Clock (HSE)** to *Crystal/Ceramic Resonator*.

### SYS

> Enable Serial Wire Debug (assuming that you are using an ST-Link-V2).

### Connectivity

> Set I2C1 and configure the speed for 400000 which is the standard 400 kHz rate.  My SSD1306 had trouble at 100 kHz.
> Set USART1 to Asynchronous and your favorite baud rate.  115200 for me.

### Middleware

None.


That is all that is necessary under Pinout & Configuration.

## Clock Configuration

The clock should not be critical for I2C and UART.

### Input Frequency
> 25

### System Clock Mux

> HSE / 1


This results in all clocks being 25 MHz except the APB1 peripheral which is 12.5 MHz max after a /2 prescaler.

## Project Manager

This is where you name the project and select your Toolchain/IDE.

The default Toolchain/IDE is **not** STM32CubeIDE so be sure to change it if that is what you plan to use.

## Last STM32CubeMX Step

> Click the *[ GENERATE CODE ]* button to generate the project.

Be sure to save your project so that you can update it later if you like.  Note that in the recent projects listing, the European date format is used:  DD/MM/YYYY.


# STM32CubeIDE

This is an Eclipse based IDE which has been configured to work with the STM32 family.  If you are not familiar with Eclipse then learn the basics.  It is used for NXP, TI, Silicon Labs, and others.  It is also very applicable for systems programming in C++ and Java.

Now open the project which was just created with the IDE.  You are primarily interested in Core/Src/main.c which contains the main() function.

Notice that the code is filled with several USER CODE comment pairs like these:

`/* USER CODE BEGIN <section> */`

`/* USER CODE END <section> */`

**NOTE:** It is important to keep your code betwen them so that if you modify a setting in STM32CubeMX, it will not wipe out your changes.


# Added Files

Open the project in the IDE.  Expand the Core folder so that you see the Inc and Src folders.  Drag-and-drop the necessary .h files into Inc and the corresponding .c files into Src.  There will be a .h and .c file for each of these:

- fonts
- ssd1306
- test

# Modifications

The ssd1306_I2C_WriteMulti() function had a hand-coded display data copy for() loop.  It was replaced with memcpy().  
The disassembly view shows that this is just 6 instructions which are inline.  

The twist() function was added to test.c just to exercise the graphics a little.

# Programming

My evaluation uses the ST-Link-V2 which allows programming and debugging.  

If your ST-Link-V2 is still in the mail, a Segger J-Link should work.   As a last resort, you can try the on-chip bootloader.


# Reference

UM1734 - User manual - STM32Cube™ USB device library

https://www.st.com/resource/en/user_manual/dm00108129-stm32cube-usb-device-library-stmicroelectronics.pdf  



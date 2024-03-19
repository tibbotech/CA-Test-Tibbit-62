# Test Application for Tibbit#62 (2-Channel 1-WIRE/SINGLE-WIRE)
Tibbit #62 offers two 1-wire/single-wire ports supporting DS18B20-based 1-wire temperature sensors and DHT11 and DHT22 single-wire temperature/humidity sensors.
For further information about Tibbit#62, you can see [Tibbit#62 documentation](https://docs.tibbo.com/tibbit_62).

This repository offers a test application that can help read multiple 1-Wire/Single-Wire sensors. 
The test application has also the capability to update the Tibbit's firware (In-system Upgrades). 

> [!TIP]
> Before using this test application, it is important to know that [AppBlocks](https://appblocks.io/), our no-code, in-browser, flowchart-based application development system for TPS supports Tibbit#62. AppBlocks automatically generates all the code required to support 1-wire and single-wire devices, including an HTML configuration page for discovering and assigning 1-wire devices connected to a TPS device. This web interface is an indispensable tool for connecting, removing, and servicing the sensors in the field. Therefore, we recommend using AppBlocks for your project; however, if you have an existing project and want to add Tibbit#62, using this application might help you significantly.


## You Will Need

- A TPP3(G2) board. You can also use any other TPP boards such as TPP2G2; you might need to change the pin assignments in *global.tbh* and change platform settings in TIDE.
- One Tibbit#62
- One Tibbit#21
- Optionally one Tibbit#10 for the power supply
- Optionally one Tibbit#18 to go along with Tibbit#10
- A few 1-Wire sensors based on DS18B20 (here, we connect only two)
- One DHT22 sensor

## Connection 
Follow the diagram below for a minimal testing setup of the Tibbit#62:
![The Block diagram of testing Tibbit#62](/Diagrams_and_Images/Connection_Diagram.png)

## The Test Program Flow
In *on_sys_init()*, 
1. The Tibbit is initialized and all voltages and faults of the Tibbit are checked.
2. CH1 of the Tibbit is set as single-wire to read a DHT22 sensor.
3. CH2 of the Tibbit is set as 1-wire to read 1-wire DS18B20-based temperature sensors.
4. The program scans CH2 for connected 1-wire sensors. This process is called discovering sensors.

> [!TIP]
> Tibbit#62 library supports both metric and imperial systems. This application uses the metric system. Suppose you want to change to an imperial system (in order to report the temperature in Fahrenheit instead of degrees Celsius). In that case, you can add *#define USE_IMPERIAL_UNITS* to your project's definitions, whether in *main.tbs* or *global.tbh*

In *on_sys_timer()*, the following process is being repeated periodically:
1. DHT22 sensor is being read periodically to report temperature and humidity in debugprint of the console.
2. A 1-wire sensor with a specific ID, distinguished by SID variable in the code, is being read and temperature is measured. You can change SID value to match one of your sensors. The sensor reading is reported in debugprint.


## In-System Updates of Tibbit Firmware
The PIC microcontroller of Tibbit #62 can be upgraded in the system and without any additional hardware. The firmware update process utilizes the low-voltageprogramming (LVP) mode of the PIC microcontroller.
Tibbo provides a library for in-system upgrades; you can find it here:
https://github.com/tibbotech/libraries/tree/main/pic

In the *on_sys_init()* event, there is a commented section. If you need to try to upgrade the Tibbit's firmware, you just need to uncomment the related section.
Your new firmware file (in hex format) should be added to the project's directory and included in the project. For testing, the latest firmware of Tibbit#62 is already added to this project, named *Tibbit62_FW_00_01.hex*.

## Need Help?
Email *support@tibbo.com*, and we will gladly help you integrate Tibbit#62 into your project.

## Useful links
* [Tibbo](https://tibbo.com)
* [Tibbo Project System (TPS)](https://tibbo.com/store/tps.html)
* [AppBlocks — our no-code platform](https://appblocks.io)
* [Tibbit #62 — Official Product Page](https://www.tibbo.com/store/tps/tibbits.html)
* [Tibbit #62 — Official Documentation](https://docs.tibbo.com/tibbit_62)
* [Tibbit #62 — Tibbo BASIC Library](https://github.com/tibbotech/libraries/tree/main/tibbits/tbt62)
* [In-System Firmware Updates Tibbo BASIC Library (LVP)](https://github.com/tibbotech/libraries/tree/main/pic)
* [Tibbo Support Center](https://tibbo.com/support.html)
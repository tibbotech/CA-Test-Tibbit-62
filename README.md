# Test Application for Tibbit#62 (2-Channel 1-WIRE/SINGLE-WIRE)
Tibbit #62 offers two 1-wire/single-wire ports supporting DS18B20-based 1-wire temperature sensors, as well as DHT11 and DHT22 single-wire temperature/humidity sensors.
For further information about Tibbit#62, you can see [Tibbit#62 documentation](https://docs.tibbo.com/tibbit_62).

This repository offers a test application that can help with reading multiple 1-Wire/Single-Wire sensors. 
The test application has also the capability to update the Tibbit's firware (In-system Upgrades). 

> [!TIP]
> Before using this test application, it is important to know that AppBlocks, our no-code, in-browser, flowchart-based application development system for TPS supports Tibbit#62. AppBlocks automatically generates all the code required to support 1-wire and single-wire devices, including an HTML configuration page for discovering and assigning 1-wire devices connected to a TPS device. This web interface is an indispensable tool for connecting, removing, and servicing the sensors in the field. Therefore, we highly recommend using AppBlocks for your project; however, if you have an existing project and you want to add Tibbit#62 to it, using this application might help you significantly.


## You Will Need

- A TPP3(G2) board. You can also use any other TPP boards such as TPP2G2; you might need to just change the pin assignments in *global.tbh* as well as changing platform settings in TIDE.
- One Tibbit#62
- One Tibbit#21
- Optionally one Tibbit#10 for power supply
- Optionally one Tibbit#18 to go along with Tibbit#10
- A few 1-Wire sensors based on DS18B20 (here we connect only two)
- One DHT22 sensor

## Connection 
Follow the diagram below for a minimal testing setup of the Tibbit#62:
![The Block diagram of testing Tibbit#62](https://github.com/tibbotech/CA-Test-Tibbit-62/Diagrams_and_Images/Connection_Diagram.png)

## The Test Program Flow
In *on_sys_init()*, 
1- Tibbit is initialized and all voltages and faults of the Tibbit are checked.
2- CH1 of the Tibbit is set as single-wire to be able to read a DHT22 sensor.
3- CH2 of the Tibbit is set as 1-wire to be able to read a 1-wire DS18B20-based temperature sensors.
4- The program scans CH2 for connected 1-wire sensors. This process is called discovery of sensors.

In *on_sys_timer()*, the following process is being repeated periodically:
1- DHT22 sensor is being read periodically to report temperature and humidity.
2- A 1-wire sensor with a specific ID, distinguished by SID variable in the code, is being read and temperature is measured. You can change SID value to match one of your sensors.

## In-system Updates of Tibbit Firmware
The PIC microcontroller of Tibbit #62 can be upgraded in the system and without any additional hardware. The firmware update process utilizes the low-voltageprogramming (LVP) mode of the PIC microcontroller.
Tibbo provides a library for in-system upgrades; you can find it here:
https://github.com/tibbotech/libraries/tree/main/pic

In the *on_sys_init()* event, there is a commented section. If you need to try to upgrade the Tibbit's firmware, you just need to uncomment the related section.
Your new firmware file (in hex format) should be added to the project's directory and included in the project. For testing, the latest firmware of Tibbit#62 is already added to this projec, named *Tibbit62_FW_00_01.hex*.

## Need Help?
Send an email to *support@tibbo.com* and we will be happy to help you with your testing.
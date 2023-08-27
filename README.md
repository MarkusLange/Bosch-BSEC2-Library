# Instructions for using the BSEC Arduino Library in Arduino 1.8.19, 2.x

## About BSEC

Bosch Sensortec Environmental Cluster (BSEC) Software v2.4.0.0 released on January 23rd, 2023

The BSEC fusion library has been conceptualized to provide a higher-level signal processing and fusion for the BME688. The library receives compensated sensor values from the sensor API. It processes the BME688 signals to provide the requested sensor outputs.

Key features

- Selectivity to target gas classes
- Calculation of index for air quality (IAQ) level outside of the device
- Calculation of ambient air temperature outside of the device (e.g. phone)
- Calculation of ambient relative humidity outside of the device

Typical applications

- Indoor air quality
- Home automation and control
- Internet of things
- Weather forecast
- GPS enhancement (e.g. time-to-first-fix improvement, dead reckoning, slope detection)
- Indoor navigation (change of floor detection, elevator detection)
- Outdoor navigation, leisure and sports applications
- Vertical velocity indication (rise/sink speed)

Supported platforms

- BSEC library is supported on 32, 16 and 8 bit MCU platforms

Available binaries for download:

| Platform | Compiler | TYPE |
|----------|----------|------|
| Cortex-ARM | GCC | Cortex-M0+, M3, M4, M4_FPU, M33_FPU, M7 |

The library size information above doesn't include additional dependencies based on the embedded system project & platform.

For other platforms, please contact your local Bosch Sensortec representative

Advantages

- Easy to integrate
- Hardware and software co-design for optimal performance
- Complete software fusion solution out of one hand
- Eliminates need for own fusion software development
- Robust virtual sensor outputs optimized for the application

## Software license agreement

The BSEC software is only available for download or use after accepting the software license agreement. By using this library, you have agreed to the terms of the license agreement.

[BSEC license agreement](https://www.bosch-sensortec.com/media/boschsensortec/downloads/software/bme688_development_software/2023_04/license_terms_bme688_bme680_bsec.pdf)

## Installation and getting started

### 1. Install the latest Arduino IDE

As of this publication, the latest Arduino IDE 1.8.19, 2.x can be downloaded from this [link](https://www.arduino.cc/en/software)

### 2. Install the BSEC2 library and BME68x Library

Download [Bosch_BSEC2_Library](https://github.com/BoschSensortec/Bosch-BSEC2-Library) and [Bosch_BME68x_Library](https://github.com/BoschSensortec/Bosch-BME68x-Library) (this library is a dependency to the BSEC2 library) as a zip and import it into the Arduino IDE. Refer to [this](https://www.arduino.cc/en/Guide/Libraries) guide on how to import libraries.

### 3. Install Teensy Board package in the Arduino IDE

- Open File->Preferences->Settings

- Insert the following link into the "Additional Boards Manager URLs":

	https://www.pjrc.com/teensy/package_teensy_index.json

	**Note:** Please ensure that the proxy settings are updated under File->Preferences->Network

- Go to Tools->Board->Boards Manager and search for "Teensy"

- Install the Teensy package

### 4. Modify the platform.txt file

If you have already used the previous example code and hack guide, remove the linker flag `-libalgobsec` in the platform.txt file and reference to the `compiler.c.elf.extra_flags`.

The standard arduino-builder now passes the linker flags under `compiler.libraries.ldflags`. Most platform.txt files do not already include this new optional variable. You will hence need to declare this variable's default and add it to the end of the combine recipe. It is recommended to declare it in the following section like below,

#### Teensy core

Original line 58,
```
## Link
recipe.c.combine.pattern="{compiler.path}{build.toolchain}{build.command.linker}" {build.flags.optimize} {build.flags.ld} {build.flags.ldspecs} {build.flags.cpu} -o "{build.path}/{build.project_name}.elf" {object_files} "{build.path}/{archive_file}" "-L{build.path}" {build.flags.libs}
```
Should be,
```
## Link
compiler.libraries.ldflags=
recipe.c.combine.pattern="{compiler.path}{build.toolchain}{build.command.linker}" {build.flags.optimize} {build.flags.ld} {build.flags.ldspecs} {build.flags.cpu} -o "{build.path}/{build.project_name}.elf" {object_files} "{build.path}/{archive_file}" {compiler.libraries.ldflags} "-L{build.path}" {build.flags.libs}
```

### 5. Verify and upload the example code

Start or restart the Arduino IDE. Open any of the example codes found under  ```Bosch_BSEC2_Library>examples```.

Select your board and COM port. Upload the example. Open the Serial monitor. You should see an output on the terminal.

### More about example codes:

- basic.ino: This is an example for illustrating the basic BSEC virtual outputs.

- basic_config_state.ino: This is an example for illustrating the BSEC feature using desired configuration setting.

- bme68x_demo_sample.ino: This demonstrator application running on an x8 board has the feature of sensor data logging and BSEC algorithm illustrated. Please refer [BME688 Development Kit-Firmware-Quick-Start-Guide](examples/bme68x_demo_sample/Quick_Start_Guide.md) for installing dependent libraries and how to flash.
	
	**Note:** Please ensure to use the bsec_interface_multi.h header file specifically, for demonstrating the multi instance feature

### 6. Tested board/core list

The current list of tested boards include,

| Core MCU | Tested boards | Arduino core version | Arduino core repository |
|----------|---------------|----------------------|-------------------------|
| Cortex-m4 | Teensy 3.0 | Teensyduino Version 1.58 for Arduino 2.x | https://github.com/PaulStoffregen/cores |
| Cortex-m4 | Teensy 3.1 | Teensyduino Version 1.58 for Arduino 2.x | https://github.com/PaulStoffregen/cores |
| Cortex-m4 | Teensy 3.2 | Teensyduino Version 1.58 for Arduino 2.x | https://github.com/PaulStoffregen/cores |
| Cortex-m4 with FPU | Teensy 3.5 | Teensyduino Version 1.58 for Arduino 2.x | https://github.com/PaulStoffregen/cores |
| Cortex-m4 with FPU | Teensy 3.6 | Teensyduino Version 1.58 for Arduino 2.x | https://github.com/PaulStoffregen/cores |
| Cortex-m7 | Teensy 4.0 | Teensyduino Version 1.58 for Arduino 2.x | https://github.com/PaulStoffregen/cores |
| Cortex-m7 | Teensy 4.1 | Teensyduino Version 1.58 for Arduino 2.x | https://github.com/PaulStoffregen/cores |

## Copyright (C) 2021 Bosch Sensortec GmbH. All rights reserved.

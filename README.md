# Rocketry Software
Software used to log data on a university student rocketry club on their L2 Certification Flight.

No charges were fired off this, it was simply to record data.

Saves telemetry to on-board microSD card as `.csv`.


## Modules used
 - Teensy 4.1
 - High G Accelerometer: H3LIS331
 - Low G Accelerometer: MPU 9250
    - Gyroscope, Magnetometer, Temperature Sensor.
 - Barometer: LPS22
    - Ambient temperature sensor
Communication with all modules over I2C.

When powering on, make sure that the MPU (Low G Accelerometer, Blue Board) has `+z` facing up to ensure good calibration.


### Software Dependencies

As can be expected for a project using many different modules, some software dependencies are required before you can compile the system. Here is the list. They will need to be installed through Arduino's IDE

 - Serial Communication Bus
    - `Wire.h`
 - High G Accelerometer uses the Adafruit H3LIS331DL Module, and requires additional libraries to function. This module is pending a replacement currently.
    - `Adafruit LIS331`
    - `Adafruit Unified Sensor`
    - `Adafruit BusIO`
 - Barometer also has similar dependencies. The Unified sensor and BusIO libraries only need to be installed once.
    - `Adafruit LPS2x`
    - `Adafruit Unified Sensor`
    - `Adafruit BusIO`
 - The MPU uses a custom library written by Wolfgang Ewald. It is by far the best in terms of signal to noise ratio and accuracy with the low g accelerometer
    - `MPU9250_WE`
 - The Teensy also has a CPU Temperature sensor. This library is custom and can be found here
    - https://github.com/LAtimes2/InternalTemperature
 - The Teensy also has an SD Card slot. This can be accessed using Arduino's built in SD library.
    - `SD.begin(BUILTIN_SDCARD);`


### Error Handling and Debugging
The Teensy 4.1 board has a built in amber flashing light. This is separate from the read/write red light. It will blink during normal operation.

If at any time during startup/initialisation that it cannot connect to a module, it will halt the boot sequence and blink the following codes:

 - Short Short Long
    - Accelerometer Error (Either one)
 - Short Long Long
    - Barometric Error
 - Short Long Short
    - SD Card Error
 - Constant Blinking
    - Initialisation done, in running normally

If it does error, power cycle after making a fix.

If a module is disconnected during normal operation, the system will continue logging frozen data or NAN values. It should not outright crash (SD Card will cause a brief system hitch).

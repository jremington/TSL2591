# TSL2591
Minimal Arduino code for configuring and reading the TSL2591 High Dynamic Range Light Sensor

This is a complicated sensor that tries to do a lot independently of an MCU, like very flexibly detecting changes of light levels and generating interrupts. It is intended to be used for estimation of ambient light intensity as detected by human vision, using an approximate mathematical model based on data from a visible and an IR photodiode. The sensor is complicated to program, and the device data sheet is poorly written and confusing.

There are two or three Arduino device libraries which try to hide that complexity, but they basically just create additional confusion, because they are themselves are quite complex and the library function descriptions are poorly written (by programmers, not users).

For detecting and quantifying low light levels, none of the interrupt flexibity or ambient illumination calculations are useful, so I wrote a minimal amount of code to initialize and configure the sensor gain and integration time, and grab data from it. I removed the obnoxious "power LED" from the Adafruit breakout module, to prevent it from interfering with low light readings. 

I am impressed by the sensitivity. It easily registers light reflected from surfaces illuminated by weak moonlight.

This code was put together with help from the VERY COMPLETE TSL2591 library by Austria Microsystems: "TSL2591MI.h" Yet Another Arduino ams TSL2591MI Lux Sensor 

https://bitbucket.org/christandlg/tsl2591mi/src/master/

https://www.arduino.cc/reference/en/libraries/tsl2591mi/

Tested with Adafruit TSL2591 Stemma breakout, after removing the obnoxious LED from the module.

SJ Remington 2/2021


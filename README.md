# TSL2591
Minimal Arduino code for configuring and reading the TSL2591 High Dynamic Range Light Sensor

This is quite a complicated sensor and tries to do an awful lot independently of an MCU, like very flexibly detecting changes of light levels and generating interrupts. It attempts to automatically estimate ambient light intensity as detected by human vision, using an approximate mathematical model based on measurements from a visible and IR photodiode. The sensor is complicated to program, and the device data sheet is poorly written and confusing.

There are two or three Arduino device libraries that try to hide that complexity, but they basically just create additional confusion, because they are themselves are quite complex and the library function descriptions are poorly written (by programmers, not users).

For just detecting raw photons, none of the interrupt flexibity or ambient illumination calculations are even useful, so I ended up writing a minimal amount of code that just configures the sensor gain and integration time, and grabs data from it. I had to remove an obnoxious "power LED" from the Adafruit module, to prevent it from interfering with low light readings. I am impressed by the sensitivity. It easily registers light reflected from surfaces illuminated by weak moonlight.

This code was put together with help from the VERY COMPLETE TSL2591 library by Austria Microsystems: "TSL2591MI.h" Yet Another Arduino ams TSL2591MI Lux Sensor 

https://bitbucket.org/christandlg/tsl2591mi/src/master/

https://www.arduino.cc/reference/en/libraries/tsl2591mi/

Tested with Adafruit TSL2591 Stemma breakout, after removing the obnoxious LED from the module.

SJ Remington 2/2021


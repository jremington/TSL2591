# TSL2591
Minimal Arduino code for configuring and reading the TSL2591 High Dynamic Range Light Sensor

This is a complicated sensor that tries to do a lot independently of an MCU, like very flexibly detecting changes of light levels and generating interrupts. It is intended to be used for estimation of ambient light intensity as detected by human vision, using an approximate mathematical model based on data from a visible and an IR photodiode. The sensor is complicated to program, and the device data sheet is poorly written and confusing.

There are two or three Arduino device libraries which try to hide that complexity, but they basically just create additional confusion, because they are themselves are quite complex and the library function descriptions are poorly written (by programmers, not users).

For detecting and quantifying low light levels, none of the interrupt flexibity or ambient illumination calculations are useful, so I wrote a minimal amount of code to initialize and configure the sensor gain and integration time, and grab data from it. I removed the obnoxious "power LED" from the Adafruit breakout module, to prevent it from interfering with low light readings. **UPDATE:**  Adafruit recently added a cuttable jumper on the back of the Stemma board to disable the LED.

I am impressed by the sensitivity. It easily registers light reflected from surfaces illuminated by weak moonlight.

This code was put together with help from the VERY COMPLETE TSL2591 library by Austria Microsystems: "TSL2591MI.h" Yet Another Arduino ams TSL2591MI Lux Sensor. This library is recommended if the more complex features of the sensor are needed, such as automatically detecting changes in illumination level. Download at: 

https://bitbucket.org/christandlg/tsl2591mi/src/master/

https://www.arduino.cc/reference/en/libraries/tsl2591mi/

Tested with Adafruit TSL2591 Stemma breakout, after removing the obnoxious LED from the module.

**CODE NOTES** 

0. The sensor does not measure lux. This code returns ONLY the raw readings from the photodiodes, which I use for measuring low light levels. Lux levels are an approximation for how the human eye sees, and can be ESTIMATED from the raw readings by a calculation that is documented elsewhere. In my opinion lux levels are not meaningful for low light intensities, in which the human eye perceives intensity only with a different response curve than color vision. If you require the lux estimates, use the TSL2591MI library linked above, which has that calculation built in.

1. Light level change detection and interrupts are not implemented.
 
2. For light level data collection, there are only two useful parameter settings for the module: gain and integration time. Use the function **TSL2591_config(gain, int_time)** to set these parameters. gain = 0 to 3 corresponds to Low, Medium, High and Maximum gain. int_time = 1 to 6 corresponds to 100 to 600 millisecond exposure.
 
3. The function **TSL2591_getData()** returns a 32 bit unsigned integer. The high order 16 bits is the visible photodiode intensity, the low order 16 bits is the IR photodiode intensity.

SJ Remington 2/2021


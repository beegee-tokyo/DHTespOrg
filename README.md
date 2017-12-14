DHTesp
===

An Arduino library for reading the DHT family of temperature and humidity sensors.    
Forked from [arduino-DHT](https://github.com/markruys/arduino-DHT)     
Original written by Mark Ruys, <mark@paracas.nl>.    

Why did I clone this library instead of forking the original repo and push the changes?
When I searched through Github for DHT libraries, I found a lot of them, some of them offers additional functions, some of them only basic temperature and humidity values. I wanted to combine all interesting functions into one library. In addition, none of the DHT libraries I found were written to work without errors on the ESP32. For ESP32 (a multi core/ multi processing SOC) task switching must be disabled while reading data from the sensor.    
Another problem I found is that many of the available libraries use the same naming (dht.h, dht.cpp), which easily leads to conflicts if different libraries are used for different platforms.    

Changes to the original library:
--------
- Renamed DHT class to DHTesp and filenames from dht.* to DHTesp.* to avoid conflicts with other libraries - beegee-tokyo, <beegee@giesecke.tk>.    
- Updated to work with ESP32 - beegee-tokyo, <beegee@giesecke.tk>.   
- Added function computeHeatIndex. Reference: [Adafruit DHT library](https://github.com/adafruit/DHT-sensor-library).    
- Added function computeDewPoint. Reference: [idDHTLib](https://github.com/niesteszeck/idDHTLib).    
- Added function getComfortRatio. Reference: [idDHTLib](https://github.com/niesteszeck/idDHTLib).    

Features
--------
  - Support for DHT11 and DHT22, AM2302, RHT03
  - Auto detect sensor model
  - Determine heat index
  - Determine dewpoint
  - Determine thermal comfort:
    * Empiric comfort function based on comfort profiles(parametric lines)
    * Multiple comfort profiles possible. Default based on http://epb.apogee.net/res/refcomf.asp
    * Determine if it's too cold, hot, humid, dry, based on current comfort profile

Functions
-----
_**`void setup(uint8_t pin, DHT_MODEL_t model=AUTO_DETECT);`**_    
Call to initialize the interface, define the GPIO pin to which the sensor is connected and define the sensor type. Valid sensor types are:     
- AUTO_DETECT     Try to detect which sensor is connected    
- DHT11    
- DHT22    
- AM2302          Packaged DHT22    
- RHT03           Equivalent to DHT22    

_**`void resetTimer();`**_    
Reset last time the sensor was read    

_**`float getTemperature();`**_    
Get the temperature in degree Centigrade from the sensor    
Either one of getTemperature() or getHumidity() initiates reading a value from the sensor if the last reading was older than the minimal refresh time of the sensor.    

_**`float getHumidity();`**_    
Get the humidity from the sensor     
Either one of getTemperature() or getHumidity() initiates reading a value from the sensor if the last reading was older than the minimal refresh time of the sensor.    

_**`DHT_ERROR_t getStatus();`**_    
Get last error if reading from the sensor failed. Possible values are:    
- ERROR_NONE      no error occured
- ERROR_TIMEOUT   timeout reading from the sensor    
- ERROR_CHECKSUM  checksum of received values doesn't match

_**`const char* getStatusString();`**_    
Get last error as a char *    

_**`DHT_MODEL_t getModel()`**_    
Get detected (or defined) sensor type    

_**`int getMinimumSamplingPeriod();`**_    
Get minimmum possible sampling period. For DHT11 this is 1000ms, for other sensors it is 2000ms    

_**`int8_t getNumberOfDecimalsTemperature();`**_    
Get number of decimals in the temperature value. For DHT11 this is 0, for other sensors it is 1    

_**`int8_t getLowerBoundTemperature();`**_    
Get lower temperature range of the sensor. For DHT11 this is 0 degree Centigrade, for other sensors this is -40 degree Centrigrade    

_**`int8_t getUpperBoundTemperature();`**_    
Get upper temperature range of the sensor. For DHT11 this is 50 degree Centigrade, for other sensors this is 125 degree Centrigrade    

_**`int8_t getNumberOfDecimalsHumidity();`**_    
Get number of decimals in the humidity value. This is always 0.    

_**`int8_t getLowerBoundHumidity();`**_    
Get lower humidity range of the sensor. For DHT11 this is 20 percent, for other sensors this is 0 percent    

_**`int8_t getUpperBoundHumidity();`**_    
Get upper temperature range of the sensor. For DHT11 this is 90 percent, for other sensors this is 100 percent    

_**`static float toFahrenheit(float fromCelcius);`**_    
Convert Centrigrade value to Fahrenheit value    

_**`static float toCelsius(float fromFahrenheit) { return (fromFahrenheit - 32.0) / 1.8; };`**_    
Convert Fahrenheit value to Centigrade value    

_**`float computeHeatIndex(float temperature, float percentHumidity, bool isFahrenheit=false);`**_    
Compute the heat index. Default temperature is in Centrigrade.    

_**`float computeDewPoint(float temperature, float percentHumidity, bool isFahrenheit=false);`**_    
Compute the dew point. Default temperature is in Centrigrade.    

_**`float getComfortRatio(ComfortState& destComfStatus, float temperature, float percentHumidity, bool isFahrenheit=false);`**_    
Compute the comfort ratio. Default temperature is in Centrigrade.    

Usage
-----
See [examples]- . For all the options, see [dhtesp.h][header].    

Installation
------------

In Arduino IDE open Sketch->Include Library->Manage Libraries then search for _**DHT ESP**_    
In PlatformIO open PlatformIO Home, switch to libraries and search for _**DHT ESP32**_. Or install the library in the terminal with _**`platformio lib install 2029`**_    

For manual installation [download] the archive, unzip it and place the DHTesp folder into the library directory.    
In Arduino IDE this is usually _**`<arduinosketchfolder>/libraries/`**_    
In PlatformIO this is usually _**`<user/.platformio/lib>`**_    
[download]: https://github.com/beegee-tokyo/DHTesp/archive/master.zip "Download DHTesp library"
[example]: https://github.com/beegee-tokyo/DHTesp/blob/master/examples "Show DHTesp examples"
[header]: https://github.com/beegee-tokyo/DHTesp/blob/master/DHTesp.h "Show header file"

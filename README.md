# homebridge-bme680

Fork from [thro's](https://github.com/trho) [homebridge-bme680](https://github.com/trho/homebridge-bme680) plugin
providing bme680 support for homebridge which introduces the following

* Fixes package.json for compatibility with latest homebridge versions
* A for of bsec_bme_680_linux to retain compatibility with this plugin

# Build Instructions

1. `git clone https://github.com/milmber/homebridge-bme680.git`
2. `cd homebridge-bme680`   
3. `npm install`
4. `npm run compile`

## Installation

1.	Install Homebridge
2.	Install this plugin `npm install -g homebridge-bme680/` (targeting the directory where this package was checked out to)
3.  Compile bsec_bme680 executable by following the guide on https://github.com/alexh-name/bsec_bme680_linux
4.  Copy `bsec_bme680` and `bsec_iaq.config` and `bsec_iaq.state` (if created) to the Homebridge storagePath (e.g. `/var/lib/homebridge`)
5.  Ensure the right permissions for the `bsec_iaq.state` file so that the Homebridge process can write to it.    
6.	Update your configuration file - see below for an example

Connect the BME680 chip to the I2C bus

## Configuration
* `accessory` "BME680"
* `name` descriptive name
* `name_temperature` (optional): descriptive name for the temperature sensor
* `name_humidity` (optional): descriptive name for the humidity sensor
* `name_air_quality` (optional): descriptive name for the air quality sensor
* `refresh` optional, time interval for refreshing data in seconds, defaults to 60 seconds.
* `options` options for [bme280-sensor](https://www.npmjs.com/package/bme280-sensor)
* `spreadsheetId` ( optional ): Log data to a Google sheet, this is part of the URL of your spreadsheet.  ie the spreadsheet ID in the URL https://docs.google.com/spreadsheets/d/abc1234567/edit#gid=0 is "abc1234567". (TODO: not implemented, yet)


Example configuration:

```json
    "accessories": [
        {
            "accessory": "BME680",
            "name": "Sensor",
            "name_temperature": "Temperature",
            "name_humidity": "Humidity",
            "useBsecLib": true,
            "bsecTempCompensation": -0.35,
            "bsecHumidityCompensation": 4.0,
            "name_air_quality": "Air Quality"
        }
    ]
```

The option `useBsecLib` only works if you placed compiled bsec_bme680 in homebridge storagePath.
By omitting this option this plugin relies on jvsbme680's sensor readings. In this case IAQ is just a dummy value.
There is an advantage using this simpler solution:
The temperature and humidity readings are much more precise in this case and no calibration & compensation is necessary in this case.
Once the compensation is this configurable I will elaborate this further.
For now see kvaruni's comment on calibration & compensation for gas readings:
https://forums.pimoroni.com/t/bme680-observed-gas-ohms-readings/6608/5


This plugin creates three services: TemperatureSensor, HumiditySensor and AirQualitySensor.

## Optional - Enable access to Google Sheets to log data

TODO: not implemented, yet

This presumes you already have a google account, and have access to google drive/sheets already

Step 1: Turn on the Drive API
a. Use this wizard ( https://console.developers.google.com/start/api?id=sheets.googleapis.com )
to create or select a project in the Google Developers Console and automatically turn on the API. Click Continue, then Go to credentials.

b. On the Add credentials to your project page, click the Cancel button.

c. At the top of the page, select the OAuth consent screen tab. Select an Email address, enter a Product name if not already set, and click the Save button.  I used 'Sheets Data Logger'

d. Select the Credentials tab, click the Create credentials button and select OAuth client ID.

e. Select the application type Other, enter the name "Drive API Quickstart", and click the Create button.

f. Click OK to dismiss the resulting dialog.

g. Click the file_download (Download JSON) button to the right of the client ID.

h. Move this file to your .homebridge and rename it logger_client_secret.json.

Step 2: Authorize your computer to access your Drive Account

a. Change to the directory where the plugin is installed i.e.

cd /usr/lib/node_modules/homebridge-mcuiot/node_modules/mcuiot-logger

b. Run the authorization module

node quickstart.js

c. Browse to the provided URL in your web browser.

If you are not already logged into your Google account, you will be prompted to log in. If you are logged into multiple Google accounts, you will be asked to select one account to use for the authorization.

d. Click the Accept button.

e. Copy the code you're given, paste it into the command-line prompt, and press Enter.

## See also

* [homebridge-bme680](https://github.com/trho/homebridge-bme680)
* [homebridge-ds18b20](https://www.npmjs.com/package/homebridge-ds18b20)
* [homebridge-dht-sensor](https://www.npmjs.com/package/homebridge-dht-sensor)
* [homebridge-dht](https://www.npmjs.com/package/homebridge-dht)
* [bsec_bme680_linux fork] https://github.com/trho/bsec_bme680_linux
* https://github.com/rxseger/homebridge-bme280
* https://github.com/jorisvervuurt/jvsbme680


## Credits

* [thro](https://github.com/trho) - for the original development of the [homebridge-bme680](https://github.com/trho/homebridge-bme680) plugin
* NorthernMan54 - Barometric Pressure and Device Polling
* simont77 - History Service
* alexh-name - C wrapper for BME680 sensor BSEC library
* jorisvervuurt - Node.js module for Pimoroni's BME680 Breakout

## License

MIT

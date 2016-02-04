Configure Arduino IDE for ESP8266
-------------------------

* See: https://github.com/esp8266/Arduino
* Remove old possibly conflicting config

```
mv ~/Library/Arduino15 ~/Library/Arduino15-old
mkdir ~/Library/Arduino15
cp ~/Library/Arduino15-old/preferences.txt ~/Library/Arduino15
```
* Start Arduino
* Click 'Arduino' -> 'Preferences'
    * Additional Boards Manager URLs: http://arduino.esp8266.com/staging/package_esp8266com_index.json
* Restart Arduino IDE
* Click 'Tools' -> 'Board' -> 'Boards Manager'
    * Select 'Generic ESP8266 Module'
* Stop Arduino IDE
* Install pyserial, esptool, and reconfigure IDE to use pyserial

```
brew install python
pip install pyserial
mkdir ~/bin
cd /tmp && git clone https://github.com/themadinventor/esptool && cp esptool/esptool.py ~/bin/esptool.py
perl -i -pe "s#/usr/bin/env python#/usr/local/bin/pythonperl -i -pe "s#/usr/bin/env python#/usr/local/bin/python#" ~/bin/esptool.py # Force esptool to use homebrew python

# Note: using command line substitution with ${HOME} below because the platform.txt file cannot interpret ~
perl -i -pe "s#tools.esptool.upload.pattern=.*#tools.esptool.upload.pattern=\"${HOME}/bin/esptool.py\" --port \"{serial.port}\" write_flash 0x00000 \"{build.path}/{build.project_name}.bin\"#" ~/Library/Arduino15/packages/esp8266/hardware/esp8266/2.1.0-rc2/platform.txt
```

* Start Arduino IDE
* Connect your ESP8266
* Upload your sketch


FTDI or PL2303 Connection
-------------------------
![](./ESP8266-01_to_FTDI.png)

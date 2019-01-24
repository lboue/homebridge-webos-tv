# homebridge-webos-tv

`homebridge-webos-tv` is a plugin for HomeBridge which allows you to control your webOS TV! It should work with all TVs that support webOS2 and newer.
The idea is that the TV should be controlled completely from the native HomeKit iOS app and Siri, that is why volume appears as a light bulb or external input as a switch.

### IMPORTANT

#### The old package ```homebridge-webos3``` is deprecated. New development will be made here. If you used the old package please change your accessory in your config.json to ```webostv``` and install this new package!

### Features
* Power status
* Turn on / off
* Mute Status (as light bulb)
* Mute / Unmute (as light bulb)
* Volume control (as light bulb and switches)
* Open apps (switch between apps of your choice and live tv)
* Channel control
* Media control
* Show notifications
* Emulate remote control

## Installation

If you are new to Homebridge, please first read the Homebridge [documentation](https://www.npmjs.com/package/homebridge).
If you are running on a Raspberry, you will find a tutorial in the [homebridge-punt Wiki](https://github.com/cflurin/homebridge-punt/wiki/Running-Homebridge-on-a-Raspberry-Pi).

Install homebridge:
```sh
sudo npm install -g homebridge
```

Install homebridge-webos-tv:
```sh
sudo npm install -g homebridge-webos-tv
```

## Configuration

Add the accessory in `config.json` in your home directory inside `.homebridge`.

```js
{
  "accessories": [
    {
      "accessory": "webostv",
      "name": "My webOS tv",
      "ip": "192.168.0.40",
      "mac": "ab:cd:ef:fe:dc:ba",
      "keyFile": "/home/pi/.homebridge/lgtvKeyFile",
      "volumeControl": "switch",
      "mediaControl": false,
      "pollingInterval": 10,
      "appSwitch":[
         "com.webos.app.tvguide",
         "com.webos.app.livetv",
         "com.webos.app.hdmi1",
         "com.webos.app.hdmi2",
         "com.webos.app.externalinput.component",
         "com.webos.netflix",
         "amazon.row",
         "googleplaymovieswebos",
         "youtube.leanback.v4"
      ],
      "channelButtons": [3,5,7,8],
      "notificationButtons": [
         "Motion detected - living room",
         "Motion detected - kitchen"
      ],
      "remoteControlButtons": [
         "HOME",
         "LIST",
         "EXIT"
      ]
    }
  ]  
}
```

You also need to enable **mobile tv on** on your tv for the turn on feature to work correctly.

On newer TVs **LG Connect Apps** under the network settings needs to be enabled.

### Configuration fields
- `accessory` [required]
Should always be "webostv"
- `name` [required]
Name of your accessory
- `ip` [required]
ip address of your tv
- `mac` [required]
Mac address of your tv
- `keyFile` [optional]
To prevent the tv from asking for permission when you reboot homebridge, specify a file path to store the permission token. If the file doesn't exist it'll be created. Don't specify a directory or you'll get an `EISDIR` error. 
- `pollingInterval` [optional]
The TV state background polling interval in seconds. **Default: 5**
- `volumeControl` [optional]
Wheter the volume control service is enabled. **Default: true**  
Available values:
  - *true* - slider and switches 
  - *"slider"* - just slider
  - *"switch"* - just switches
- `volumeLimit` [optional]
The max allowed volume which can be set using the volume service. Range 1-100. **Default: 100**
- `channelControl` [optional]
Wheter the channel control service is enabled. **Default: true**
- `mediaControl` [optional]
Wheter the media control service is enabled. Buttons: play, pause, stop, rewind, fast forward. **Default: true**
- `appSwitch` [optional] 
Wheter the app switch service is enabled. This allows to switch live tv with apps of your choice. To get the app ID simply open an app on your TV and check the homebridge console. The app ID of the opened app will be printed. **Default: "" (disabled)**
  - Set an array of app IDs as the value
  - External sources are also apps and can be used as app switches, available sources:
    - *com.webos.app.livetv*
    - *com.webos.app.hdmi1*, 
    - *com.webos.app.hdmi2*, 
    - *com.webos.app.hdmi3*, 
    - *com.webos.app.externalinput.component*, 
    - *com.webos.app.externalinput.av1*
  - Apps can also be started when the TV is off, in that case an attempt to power on the TV and switch to the chosen app will be made
- `channelButtons` [optional] 
Wheter the channel buttons service is enabled. This allows to create switches for the channels of your choice. This way you can quickly switch between favorite channels. **Default: "" (disabled)**
  - Set an array of channel numbers as the value
  - Channels can also be opened when the TV is off, in that case an attempt to power on the TV and afterwards open the chosen channel will be made.
- `notificationButtons` [optional] 
Wheter the notification buttons service is enabled. This allows to create buttons which when pressed display the specified text on the TV screen. Useful for HomeKit automation or to display text on TV for viewers. **Default: "" (disabled)**
  - Set an array of notification texts as the value
- `remoteControlButtons` [optional] 
Wheter the remote control buttons service is enabled. This allows to emulate remote control buttons. **Default: "" (disabled)**
  - Set an array of commands as the value. Possible values are:
    - *1*, *2*, *3*, *4*, *5*, *6*, *7*, *8*, *9*, *0*, *LIST*, *AD*, *DASH*
    - *MUTE*, *VOLUMEUP*, *VOLUMEDOWN*, *CHANNELUP*, *CHANNELDOWN*, *HOME*, *MENU*
    - *UP*, *DOWN*, *LEFT*, *RIGHT*, *CLICK*, *BACK*, *EXIT*, *PROGRAM*, *ENTER*
    - *RED*, *GREEN*, *YELLOW*, *BLUE*, *LIVE_ZOOM*, *CC*
  - Most probably there are also other values possible which i didn't find yet (like settings or voice command), you can try typing some other values and if you find some that work then please let me know
  
## Troubleshooting
If you have any issues with the plugin or tv services then you can run homebridge in debug mode, which will provide some additional information. This might be useful for debugging issues. 

Homebridge debug mode:
```sh
homebridge -D
```

## Special thanks
[lgtv2](https://github.com/hobbyquaker/lgtv2) - the Node.js remote control module for LG WebOS smart TVs.

[homebridge-lgtv2](https://github.com/alessiodionisi/homebridge-lgtv2) & [homebridge-webos2](https://github.com/zwerch/homebridge-webos2) - the basic idea for the plugin.

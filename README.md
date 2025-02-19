<span align="center">

# homebridge-xiaomi-fan-de

[![verified-by-homebridge](https://badgen.net/badge/homebridge/verified/purple)](https://github.com/homebridge/homebridge/wiki/Verified-Plugins#verified-plugins)
[![homebridge-xiaomi-fan](https://badgen.net/npm/v/homebridge-xiaomi-fan?icon=npm)](https://www.npmjs.com/package/homebridge-xiaomi-fan)
[![mit-license](https://badgen.net/npm/license/lodash)](https://github.com/merdok/homebridge-xiaomi-fan/blob/master/LICENSE)
[![follow-me-on-twitter](https://badgen.net/twitter/follow/merdok_dev?icon=twitter)](https://twitter.com/merdok_dev)
[![join-discord](https://badgen.net/badge/icon/discord?icon=discord&label=homebridge-xiaomi-fan)](https://discord.gg/AFYUZbk)

</span>

This is a fork from the original Project, just translated to german

`homebridge-xiaomi-fan` is a plugin for homebridge which allows you to control Xiaomi Smartmi and Mija Fans! It should work with most smart fans from xiaomi.
The goal is to make the fan fully controllable from the native Homekit iOS app and Siri.

### Features
* Integrates into homekit as a fan device
* Control power, speed, swing mode and switch between standard and natural wind
* Set oscillation angle
* Rotate fan to the left or right by 5°
* Turn on/off the buzzer
* Turn on/off the LEDs
* Set a shutdown timer
* Homekit automations

### Known supported fan models
* zhimi.fan.sa1 (Mi Standing Fan)
* zhimi.fan.v2/v3 (Smartmi DC Pedestal Fan)
* zhimi.fan.za1 (Smartmi Inverter Pedestal Fan)
* zhimi.fan.za3 (Smartmi Standing Fan 2)
* zhimi.fan.za4 (Smartmi Standing Fan 2S)
* zhimi.fan.za5 (Smartmi Standing Fan 3)
* zhimi.fan.fa1 (Mijia DC Circulating Fan)
* zhimi.fan.fb1 (Mi Smart Air Circulator Fan)
* dmaker.fan.1c (Mi Smart Standing Fan 1C)
* dmaker.fan.p5 (Mi Smart Standing Fan 1X)
* dmaker.fan.p8 (Mi Smart Standing Fan 1C CN)
* dmaker.fan.p9 (Mi Smart Tower Fan)
* dmaker.fan.p10 (Mi Smart Standing Fan 2)
* dmaker.fan.p11 (Mi Smart Standing Fan Pro)
* dmaker.fan.p15 (Mi Smart Standing Fan Pro EU)
* dmaker.fan.p18 (Mi Smart Fan 2)
* air.fan.ca23ad9 (AIRMATE CA23-AD9 Air Circulation Fan)
* dmaker.fan.p30 (Xiaomi Smart Standing Fan 2)
* dmaker.fan.p33 (Xiaomi Smart Standing Fan 2 Pro)
* dmaker.fan.p220 (Mijia DC Inverter Circulating Floor Fan)

## Installation

If you are new to homebridge, please first read the homebridge [documentation](https://www.npmjs.com/package/homebridge).
If you are running on a Raspberry, you will find a tutorial in the [homebridge wiki](https://github.com/homebridge/homebridge/wiki/Install-Homebridge-on-Raspbian).

Install homebridge:
```sh
sudo npm install -g homebridge
```

Install homebridge-xiaomi-fan:
```sh
sudo npm install -g homebridge-xiaomi-fan
```

## Configuration

Add the `xiaomifan` platform in `config.json` in your home directory inside `.homebridge`.

Add your Fan or multiply Fans in the `devices` or `fans`  array.

Example configuration:

```js
{
  "platforms": [
    {
      "platform": "xiaomifan",
      "devices": [
        {
          "name": "Xiaomi Fan 2s",
          "ip": "192.168.0.40",
          "token": "8305d8fba83f94bb5ad8f963b6c84c84",
          "pollingInterval": 10,
          "moveControl": true,
          "buzzerControl": true,
          "ledControl": true,
          "naturalModeControl": true,
          "shutdownTimer": true,
          "angleButtons": [
             5,
             60,
             100
           ]
        }
      ]
    }
  ]
}
```

### Token
For the plugin to work the device token is required. For methods on how to find the token refer to this guide [obtaining mi device token](https://github.com/jghaanstra/com.xiaomi-miio/blob/master/docs/obtain_token.md).

You can also use this tool to easily retrieve the token: [Xiaomi Cloud Tokens Extractor](https://github.com/PiotrMachowski/Xiaomi-cloud-tokens-extractor).

### Configuration
#### Platform Configuration fields
- `platform` [required]
Should always be **"xiaomifan"**.
- `devices` or `fans` [required]
A list of your Fans.
#### Fan Configuration fields
- `name` [required]
Name of your accessory.
- `ip` [required]
ip address of your Fan.
- `token` [required]
The device token of your Fan.
- `deviceId` [optional]
New fan devices which use the miot protocol require the device id to be specified. The deviceId will be automatically retrieved by the plugin but if there is trouble you can manually specify it. **Default: "" (not specified)**
- `model` [optional]
The fan model. If specified then the accessory will be created instantly without the need to first discover and identify the fan. **Default: "" (not specified)**
- `prefsDir` [optional]
The directory where the fan device info will be stored. **Default: "~/.homebridge/.xiaomiFan"**
- `pollingInterval` [optional]
The fan state background polling interval in seconds. **Default: 5**
- `deepDebugLog` [optional]
Enables additional more detailed debug log. Useful when trying to figure out issues with the plugin. **Default: false**
- `buzzerControl` [optional]
Whether the buzzer service is enabled. This allows to turn on/off the fan buzzer. On Smartmi fans the rotation direction switch can be used to select between loud or quiet buzzer level. **Default: true**
- `ledControl` [optional]
Whether the led service is enabled. This allows to turn on/off the fan LED. **Default: true**
- `naturalModeControl` [optional]
Show a switch which allows to quickly enable/disable the natural mode. Only on supported devices! **Default: true**
- `sleepModeControl` [optional]
Show a switch which allows to quickly enable/disable the sleep mode. Only on supported devices! **Default: true**
- `moveControl` [optional]
Whether the move service is enabled. This allows to move the fan in 5° to the left or right. Only on supported devices! **Default: false**
- `shutdownTimer` [optional]
Show a slider (as light bulb) which allows to set a shutdown timer in minutes. **Default: false**
- `angleButtons` [optional]
Whether the angle buttons service is enabled. This allows to create buttons which can switch between different oscillation angles. Only on supported devices! **Default: "" (disabled)**
  - Set an array of numeric values. Possible values depend on the fan model
  - Some fans support predefined angle buttons, in the case if the property is not specified the angle buttons are retrieved from the fan and displayed as switches. If you want to prevent that behaviour set the property value as an empty array **[]** or **false**
  - Tapping the active oscillation angle button will disable oscillation completely
- `verticalAngleButtons` [optional]
Whether the vertical angle buttons service is enabled. This allows to create buttons which can switch between different vertical oscillation angles. Only on supported devices! **Default: "" (disabled)**
  - Set an array of numeric values. Possible values depend on the fan model
  - Some fans support predefined vertical angle buttons, in the case if the property is not specified the vertical angle buttons are retrieved from the fan and displayed as switches. If you want to prevent that behaviour set the property value as an empty array **[]** or **false**
  - Tapping the active vertical oscillation angle button will disable vertical oscillation completely
- `ioniserControl` [optional]
Show a switch which allows to quickly enable/disable the ioniser on your fan. Only on supported devices! **Default: false**
- `fanLevelControl` [optional]
Show fan level switches which allow to change the fan level. Only on supported devices! **Default: true**

## Troubleshooting
If you have any issues with the plugin or fan services then you can run homebridge in debug mode, which will provide some additional information. This might be useful for debugging issues.

Homebridge debug mode:
```sh
homebridge -D
```

Deep debug log, add the following to your config.json:
```json
"deepDebugLog": true
```
This will enable additional extra log which might be helpful to debug all kind of issues.

## Special thanks
[miio](https://github.com/aholstenson/miio) - the Node.js remote control module for Xiaomi Mi devices.

[HAP-NodeJS](https://github.com/KhaosT/HAP-NodeJS) & [homebridge](https://github.com/nfarina/homebridge) - for making this possible.

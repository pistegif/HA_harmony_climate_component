## Harmony Hub Climate Controller

[![homeassistant_community](https://img.shields.io/badge/HA%20community-forum-brightgreen)](https://community.home-assistant.io/t/harmony-hub-climate-component-for-a-c-integration/76793)
[![Github Stars](https://img.shields.io/github/stars/so3n/HA_harmony_climate_component)](https://github.com/so3n/HA_harmony_climate_component)

Harmony Hub Climate Controller allows you to control IR climate devices (eg. split system air conditioners) through a Harmony Hub. 

This component appears to home assistant as a climate device and as such can be intuitively used to control an air conditioner or other climate device.

<img style="border: 1px solid #767676;border-radius: 10px;max-width: 460px;width: 100%;box-sizing: border-box;" src="https://raw.githubusercontent.com/so3n/HA_harmony_climate_component/master/img/thermostat_card.png" alt="Thermostat Lovelace Card">

I forked from this [project](https://github.com/vpnmaster/homeassistant-custom-components), which was created for Broadlink RM Devices, so thanks goes to [vpnmaster](https://github.com/vpnmaster) for doing the hard work in creating that component.

## Installing

If not installing via [HACS](https://hacs.xyz/), follow the manual steps below:
1. Download or clone this project, and place the `custom_components` folder and its contents into your Home Assistant config folder.
2. Ensure `climate.py` is located in a folder named `harmony_ac` within the `custom_components` folder.


## Configuration.yaml
| Variable | Optional | Description |
| -------- | ---------- | ----------- |
| platform | Required | set this to `harmony_ac` |
| name | Optional | Name you would like to give this climate component |
| remote_entity | Required |  `entity_id` of your existing harmony device in HA that will send the commands |
| device_id | Required |  The ID Harmony has assigned to your air conditioner unit (see further below for how to obtain) |
| min_temp | Optional |  Set minimum temperature range (default: 16) |
| max_temp | Optional |  Set maximum temperature range (default: 30) |
| target_temp | Optional |  Set initial target temperature. (default: 20) |
| target_temp_step | Optional |  set target temperature step. (default: 1) |
| temp_sensor | Optional |  `entity_id` of a temperature sensor to display room temperature <br/>_temp_sensor.state must be temperature_ |
| customize  | Optional |  List of options to customize
  | - operations | Optional |  List of operation modes (default: heat, cool, auto) <br/>_do not include the OFF mode in this list_
  | - fan_modes | Optional |  List of fan modes (default: auto, low, mid, high)
  
### Example
```yaml
climate:
  - platform: harmony_ac
    name: Living Room
    remote_entity: remote.living_room
    device_id: 12345678
    min_temp: 18
    max_temp: 30
    target_temp: 20
    target_temp_step: 0.5
    temp_sensor: sensor.living_room_temp
    customize:
      operations:
        - cool
        - heat
        - dry
        - auto
      fan_modes:
        - auto
        - low
        - mid
        - high
```

## How to obtain your Air Conditioner's Device ID

This assumes you have already setup the official home assitant [harmony component](https://www.home-assistant.io/components/remote.harmony/) and have added your air conditioner as a device in the [MyHarmony](https://www.myharmony.com) software.
* in your home assistant config folder, delete `harmony_*.conf` if it exists. eg. mine is called `harmony_living_room.conf`
* restart home assistant
* when it boots up it should create a new `harmony_*.conf` file. Open this and find your air conditioner device, it should have an ID number next to it.

## How to learn and name all the IR commands for your air conditioner
This part is unfortunately going to be manual. Every combination of **operations** (heat, cool, dry, etc), **fan modes** (low, mid, high, auto, etc) and **temperatures**, will need to be learned manually within the MyHarmony software. The naming convention of each command is important for this component to work.

* in MyHarmony, go to devices > your air conditioner and click on **Add or Fix a Command**
* Click on **add a missing command** and enter a name in the following format: *OperationFanmodeTemperature* eg. *CoolHigh18* and then follow the prompts within MyHarmony
    
some important notes about the naming convention for commands:
* capitalise the first letter of the *operation* and *fan-mode*
* **Operation** must be one of the operations listed in your configuration.yaml file
* **Fanmode** must be one of the fan_modes listed in your configuration.yaml file
* **Temperature** must be an integer in the range specified by your min/max temp in configuration.yaml file
* The only exception to these rules is the 'off' command. Just name this as **Off** in MyHarmony (note the capitalised first letter)

Some valid examples of command names based on the above configuration.yaml example
```
Off
CoolAuto18
CoolAuto19
CoolAuto20
...
CoolAuto30
CoolHigh18
CoolHigh19
CoolHigh20
...
CoolHigh30
HeatLow18
HeatLow19
etc
etc 
```

<hr>

<a href="https://www.buymeacoffee.com/so3n" target="_blank"><img src="https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png" alt="Buy Me A Coffee" style="height: auto !important;width: auto !important;" ></a>
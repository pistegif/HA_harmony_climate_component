## Harmony Hub Climate Controller

[![homeassistant_community](https://img.shields.io/badge/HA%20community-forum-brightgreen)](https://community.home-assistant.io/t/harmony-hub-climate-component-for-a-c-integration/76793) [![Github Stars](https://img.shields.io/github/stars/so3n/HA_harmony_climate_component)](https://github.com/so3n/HA_harmony_climate_component) 
[![Buy Me A Coffee](https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png)](https://www.buymeacoffee.com/so3n)

Harmony Hub Climate Controller allows you to control IR climate devices (eg. split system air conditioners) through a Harmony Hub. 

This component appears to home assistant as a climate device and as such can be intuitively used to control an air conditioner or other climate device.

![Thermostat Lovelace Card](https://raw.githubusercontent.com/so3n/HA_harmony_climate_component/master/img/thermostat_card.png)

I forked from this [project](https://github.com/vpnmaster/homeassistant-custom-components), which was created for Broadlink RM Devices, so thanks goes to [vpnmaster](https://github.com/vpnmaster) for doing the hard work in creating that component.

## Installing

Recommended install via [HACS](https://hacs.xyz/), otherwise follow the manual steps below:
1. Download or clone this project, and place the `custom_components` folder and its contents into your Home Assistant config folder.
2. Ensure `climate.py` is located in a folder named `harmony_ac` within the `custom_components` folder.


## Configuration
Once this custom component is installed, add the following to your `configuration.yaml` to use it in your HA installation

```yaml
climate:
  - platform: harmony_ac
    remote_entity: remote.living_room
    device_id: 12345678
```
*** _refer below how to obtain `device_id`_ and for all the configuration options

### Main Configuration Options
 
|Variable        |Type   |Required|Default                   |Description                                                                                                            |
|----------------|-------|--------|--------------------------|-----------------------------------------------------------------------------------------------------------------------|
|name            |string |FALSE   |Harmony Climate Controller|Name you would like to give this climate component                                                                     |
|remote_entity   |string |TRUE    |                          |`entity_id` of your existing harmony device in HA that will send the IR commands                                       |
|device_id       |integer|TRUE    |                          |The ID which Harmony has assigned to the climate device you wish to control<br/>(refer to FAQ's below on how to obtain)|
|min_temp        |float  |FALSE   |16                        |Set minimum temperature range                                                                                          |
|max_temp        |float  |FALSE   |30                        |Set maximum temperature range                                                                                          |
|target_temp     |float  |FALSE   |20                        |Set initial target temperature                                                                                         |
|target_temp_step|float  |FALSE   |1                         |Set target temperature step                                                                                            |
|temp_sensor     |string |FALSE   |                          |`entity_id` for a temperature sensor, target_sensor.state must be temperature                                          |
|customize       |list   |FALSE   |                          |List of options to customize. Refer to table below                                                                     |

### `Customize`Configuration Options

|Variable  |Type|Required|Default                              |Description                                                                                                             |
|----------|----|--------|-------------------------------------|------------------------------------------------------------------------------------------------------------------------|
|operations|list|FALSE   |- heat<br/>- cool<br/>- auto         |List of operation modes (nest under `customize`)<br/>_do not include the OFF mode in this list_                         |
|fan_modes |list|FALSE   |- auto<br/>- low<br/>- mid<br/>- high|List of fan modes (nest under `customize`)                                                                             |
  
### Example Usage
```yaml
climate:
  - platform: harmony_ac
    name: Living Room
    remote_entity: remote.living_room
    device_id: 12345678
    min_temp: 18
    max_temp: 30
    target_temp: 20
    target_temp_step: 1
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

## FAQ's

### How to obtain your Air Conditioner's Device ID
This assumes you have already setup the official home assitant [harmony component](https://www.home-assistant.io/components/remote.harmony/) and have added your air conditioner as a device in the [MyHarmony](https://www.myharmony.com) software.
* in your home assistant config folder, delete `harmony_*.conf` if it exists. eg. mine is called `harmony_living_room.conf`
* restart home assistant
* when it boots up it should create a new `harmony_*.conf` file. Open this and find your air conditioner device, it should have an ID number next to it.

### How to learn and name all the IR commands for your air conditioner
This part is unfortunately going to be manual. Every combination of **operations** (heat, cool, dry, etc), **fan modes** (low, mid, high, auto, etc) and **temperatures**, will need to be learned manually within the MyHarmony software. The naming convention of each command is important for this component to work.

* in MyHarmony, go to devices > your air conditioner and click on **Add or Fix a Command**
* Click on **add a missing command** and enter a name in the following format: *OperationFanmodeTemperature* eg. *CoolHigh18* and then follow the prompts within MyHarmony
    
some important notes about the naming convention for commands:
* **Operation** must be one of the operations listed in your configuration.yaml file
* **Fanmode** must be one of the fan_modes listed in your configuration.yaml file
* **Temperature** must be an integer in the range specified by your min/max temp in configuration.yaml file
* The only exception to these rules is the 'off' command. Just name this as **Off** in MyHarmony

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
[![Buy Me A Coffee](https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png)](https://www.buymeacoffee.com/so3n)

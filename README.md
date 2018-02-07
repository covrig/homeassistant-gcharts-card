# Extra Google charts for [Home Assistant](https://home-assistant.io)

<img src="https://i.imgur.com/XSTSlds.gif" height="500">

KNOWN PROBLEMS: some 

## Installation
* Download `/www/custom_ui/state-card-gchart.html` to `<your-hass-configuration-dir>/www/custom_ui/` (create the folder structure if you don't have it - mind the permissions)
* Add it to your `configuration.yaml`:
```yaml
frontend:
  extra_html_url:
    - /local/custom_ui/state-card-gchart.html
```
* Create one or more sensors, binary_sensor(s), input_text(s) etc. in your `configuration.yaml`. E.g.:
```yaml
sensor:
  - platform: template
    sensors:
      gchart:
        value_template: gchart
      gchart2:
        value_template: gchart
```
* Add your sensor to a group.. E.g.:
```yaml
group:
  group_gchart:
    name: ' '   > in this format the iframe will not have a name above
    entities:
      - sensor.gchart
  group_ghcart2:
    name: 'Name'   > with a name
    entities:
      - sensor.gchart2
```
* Customize your newly created sensor in the `customize` section or your `customize.yaml` file:

```yaml
  customize:
    sensor.gchart:
      custom_ui_state_card: state-card-gchart
    sensor.gchart2:
      custom_ui_state_card: state-card-gchart

 ```

## Changelog
```
Version 20180207:
Start
```

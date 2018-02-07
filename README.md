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
* Convert your newly created sensor to a Google chart in the `customize` section or your `customize.yaml` file:

```yaml
  customize:
    sensor.gchart:
      custom_ui_state_card: state-card-gchart
    sensor.gchart2:
      custom_ui_state_card: state-card-gchart

 ```
 * Customize even your newly created sensor by edititing the `state-card-gchart.html` file (more comments in the file):

Main idea.
1. Add your entities as variables in the designated section.
```
      var pc = parseInt(this.hass.states['sensor.pc_power'].state);
      var oven = parseInt(this.hass.states['sensor.oven_power'].state);
      var washer = parseInt(this.hass.states['sensor.washer_power'].state);
      var other = parseInt(this.hass.states['sensor.power_other'].state);
      ...
 ```
 2. Add/delete rows to/from your `data` array (mind the comma after the each row):
 ```
       var data = google.visualization.arrayToDataTable([
        ['Device', 'Power',{ role: 'annotation' },  { role: 'style' }],
        ['PC', pc, pc, '#e31a1c'],
        ['Oven', oven, oven, '#fdbf6f'],
        ['Washer', washer, washer, 'color: #ff7f00' ],
        ['Other', other, other, '#cab2d6'],
        ...
      ]);
 ```

## Changelog
```
Version 20180207:
Start
```

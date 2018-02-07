# Google Charts state card for [Home Assistant](https://home-assistant.io)

<img align="left" src="https://i.imgur.com/XSTSlds.gif" height="350">

KNOWN PROBLEMS: the code could be cleaner, the PieChart needs a bit more work, the more-info card is disabled with a trick...

_NO NEED TO RESTART HASS EVERY TIME YOU CHANGE AN OPTION. TO TEST YOUR CHANGES JUST CLEAR YOUR CACHE OR USE A NEW INCONGNITO WINDOW (AFTER EACH CHANGE)._

I didn't create any config options under the `customize` section since the option list is extremly long.

I will work in adding all chart types. Maybe others will contribute.


## Features
* Updates in real time;
* Sortable by name or value;
* Chart type can be changed on the fly (3 default options: column, bar, piechart - more can ba added: read the comments in the `state-card-gchart.html` file);
* Supports one more more entities (recomended to compare: power, temperature, humidity, mold etc.).
* Could be also use as a weather forecast (disabling some of the options might be needed).
* Highly customizable (styling, axes types, annotations etc.). Visit [Google Charts](https://developers.google.com/chart/interactive/docs/gallery) for more information and options ([ColumnChart](https://developers.google.com/chart/interactive/docs/gallery/columnchart), [BarChart](https://developers.google.com/chart/interactive/docs/gallery/barchart), [PieChart](https://developers.google.com/chart/interactive/docs/gallery/piechart)).
* The height of the chart can be set in the `options` section in `state-card-gchart.html`

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
    name: ' '   > in this format the chart will not have a name above
    entities:
      - sensor.gchart
  group_ghcart2:
    name: 'Name'   > with a name (large group name, not recommended)
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
 3. Lots of options to change (a lot more can be added):
 ```
      var options = {
        title: "Instant power in W",
        height: 300,                                     //chart heigh in pixels
        bar: {groupWidth: '69%'},                        //bar/column width - 69% is the golden ratio
        legend: { position: 'none'},                     //if enabled the chartArea option should be modified
        titleTextStyle: {fontSize: 13, bold: false},
        chartArea: {left:35,top:25,right: 5, width:'100%',height:'82%'},
        tooltip: {trigger: 'selection'},                // 'focus'/'selection'/'none'
        vAxis: {minorGridlines: {count: 2, color: '#F4F4F4'}, gridlines: {count: 6}},       //for the column chart 
        hAxis: {minorGridlines: {count: 2, color: '#F4F4F4'}, gridlines: {count: 6}},       //font the bar chart
        // just a commnent for the vAxis/hAxis - if gridlines: {count: x} is set to x=-1(auto) the axis maximum value will change and the chart bars will be most of the time static. I recomend setting it to 5 or more.
        dataOpacity: 0.9,
        fontSize: 11,                                   //font size throughout the chart
        pieHole: 0.4,                                   //from pie to donut - comment or set to 0 to change to pie 
        pieSliceText: 'label',                          //'percentage', 'value' ,'label' ,'none'
        pieSliceTextStyle: {fontSize:10},
        is3D: false,                                     // set to true for 3D pie chart
        backgroundColor: { fill: 'white' },
        //explorer: {}                                   // uncomment to enable pan and zoom in the chart - right click resets
      };
 ```
## Changelog
```
Version 20180207:
Start
```

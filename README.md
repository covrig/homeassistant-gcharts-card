# WebSocket API Google Charts for [Home Assistant](https://home-assistant.io)

<img align="left" src="https://i.imgur.com/XSTSlds.gif" height="350">

***
KNOWN PROBLEMS: the code could be cleaner, the PieChart needs a bit more work, the more-info card is disabled with a trick, a bit tricky to add more than one chart ([details at the end of this page](https://github.com/covrig/homeassistant-gcharts-card/blob/master/README.md#how-to-add-more-then-one-chart))...

_No need to restart hass every time you change an option. To test your changes just clear your cache or use a new incongnito window (after each change).._

I didn't create any config options under the `customize` section since the option list is extremly long.

I will work in adding all chart types. Maybe others will contribute.
***


## Features
* Updates in real time;
* Sortable by name or value;
* Chart type can be changed on the fly (3 default options: column, bar, piechart - more can ba added: read the comments in the `state-card-gchart.html` file);
* Supports one more more entities (recomended to compare: power, temperature, humidity, mold etc.).
* Could be also use as a weather forecast (disabling some of the options might be needed).
* Highly customizable (styling, axes types, annotations etc.). Visit [Google Charts](https://developers.google.com/chart/interactive/docs/gallery) for more information and options ([ColumnChart](https://developers.google.com/chart/interactive/docs/gallery/columnchart), [BarChart](https://developers.google.com/chart/interactive/docs/gallery/barchart), [PieChart](https://developers.google.com/chart/interactive/docs/gallery/piechart)).
* The height of the chart can be set in the `options` section in `state-card-gchart.html`

<p align="center">
<img src="https://i.imgur.com/HlveuIS.jpg" height="350">
</p>

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
```
* Add the sensor to your group or a new one.. E.g.:
```yaml
group:
  group_gchart:
    name: ' '   > in this format the chart will not have a name above (recommeded)
    entities:
      - sensor.gchart
```
or
```
  group_ghcart:
    name: 'Name'   > with a name (large group name, not recommended)
    entities:
      - sensor.gchart
```
* Convert your newly created sensor to a Google chart in the `customize` section or your `customize.yaml` file:

```yaml
  customize:
    sensor.gchart:
      custom_ui_state_card: state-card-gchart
 ```
 * Customize even more your newly created sensor by edititing the `state-card-gchart.html` file (more comments in the file):

**Main idea.**
1. Add your entities as variables in the designated section.
```
      var pc = parseFloat(this.hass.states['sensor.pc_power'].state);         \\parseInt() is also an option
      var oven = parseFloat(this.hass.states['sensor.oven_power'].state);
      var washer = parseFloat(this.hass.states['sensor.washer_power'].state);
      var other = parseFloat(this.hass.states['sensor.power_other'].state);
      ...
 ```
 2. Add/delete rows to/from your `data` array (mind the comma after the each row):
 ```javascript
       var data = google.visualization.arrayToDataTable([
        ['Device', 'Power',{ role: 'annotation' },  { role: 'style' }],
        ['PC', pc, pc, '#e31a1c'],
        ['Oven', oven, oven, '#fdbf6f'],
        ['Washer', washer, washer, '#ff7f00' ],
        ['Other', other, other, '#cab2d6'],
        ...
      ]);
 ```
 3. Lots of options to change (a lot more can be added):
 ```javascript
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
## How to add more then one chart
* Duplicate `state-card-gchart.html`;
* Change the name of `state-card-gchart.html` to `state-card-whateveryouwant.html`;
* Inside `state-card-whateveryouwant.html` rename `state-card-gchart` to `state-card-whateveryouwant`(it appears to times);
* Inside `state-card-whateveryouwant.html` rename `class Chart ...` to `class YourChoice ...` and `customElements.define(Chart.is, Chart);` (last row) to `customElements.define(YourChoice.is, YourChoice);`
* Follow the same steps as for the first state chart (see above) using `state-card-whateveryouwant` instead of `state-card-gchart.html`;
* Third chart... same steps.

## Changelog
```
2018.Feb.08:
- [x] Some code cleaning. 
- [x] Updated parseInt() to parseFloat().
2019.Jan.09:
- [x] Added websocket version. Will go obsolete when the legacy password will be removed. Same approach for installation inside Home Assistant. The html will also work independetly - without Home Assistant. Just add your IP and legacy password.
```

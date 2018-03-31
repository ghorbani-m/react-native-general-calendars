# React Native General Calendars ✨ 🗓️ 📆
[![NPM Version](https://img.shields.io/npm/v/react-native-general-calendars.svg?style=flat)](https://www.npmjs.com/package/react-native-general-calendars)
[![Build Status](https://travis-ci.org/rghorbani/react-native-general-calendars.svg?branch=master)](https://travis-ci.org/rghorbani/react-native-general-calendars)

This module includes various customizable react native calendar components.

The package is both **Android** and **iOS** compatible.

## Installation

`$ yarn add react-native-general-calendars`

or

`$ npm install react-native-general-calendars --save`

The solution is implemented in JavaScript so no native module linking is required.

## Usage

`import {` [Calendar](#calendar), [CalendarList](#calendarlist) `} from 'react-native-general-calendars';`

All parameters for components are optional. By default the month of current local date will be displayed.

Event handler callbacks are called with `calendar objects` like this:

```javasctipt
{
  day: 1,     // day of month (1-31)
  month: 1,   // month of year (1-12)
  year: 2017, // year
  timestamp,   // UTC timestamp representing 00:00 AM of this date
  dateString: '2016-05-13' // date formatted as 'YYYY-MM-DD' string
}
```

Parameters that require date types accept YYYY-MM-DD formated datestrings (in gregorian), JavaScript date objects, `calendar objects` and UTC timestamps.

Calendars can be localized by adding custom locales to `LocaleConfig` object:

```javascript
import {LocaleConfig} from 'react-native-calendars';

LocaleConfig.locales['fr'] = {
  monthNames: ['Janvier','Février','Mars','Avril','Mai','Juin','Juillet','Août','Septembre','Octobre','Novembre','Décembre'],
  monthNamesShort: ['Janv.','Févr.','Mars','Avril','Mai','Juin','Juil.','Août','Sept.','Oct.','Nov.','Déc.'],
  dayNames: ['Dimanche','Lundi','Mardi','Mercredi','Jeudi','Vendredi','Samedi'],
  dayNamesShort: ['Dim.','Lun.','Mar.','Mer.','Jeu.','Ven.','Sam.']
};

LocaleConfig.defaultLocale = 'fr';
```

### Calendar

<kbd>
  <img src="https://github.com/rghorbani/react-native-general-calendars/blob/master/demo/calendar.gif?raw=true">
</kbd>

#### Basic parameters

```javascript
<Calendar
  // Calendar type (gregorian, jalaali). Default = gregorian
  type={'gregorian'}
  // Initially visible month. Default = Date()
  current={'2012-03-01'}
  // Minimum date that can be selected, dates before minDate will be grayed out. Default = undefined
  minDate={'2012-05-10'}
  // Maximum date that can be selected, dates after maxDate will be grayed out. Default = undefined
  maxDate={'2012-05-30'}
  // Handler which gets executed on day press. Default = undefined
  onDayPress={(day, localDay) => {console.log('selected day', day, localDay)}}
  // Handler which gets executed on day long press. Default = undefined
  onDayLongPress={(day) => {console.log('selected day', day)}}
  // Month format in calendar title. Formatting values: http://arshaw.com/xdate/#Formatting
  monthFormat={'YYYY MM'}
  // Handler which gets executed when visible month changes in calendar. Default = undefined
  onMonthChange={(month) => {console.log('month changed', month)}}
  // Hide month navigation arrows. Default = false
  hideArrows={true}
  // Replace default arrows with custom ones (direction can be 'left' or 'right')
  renderArrow={(direction) => (<Arrow />)}
  // Do not show days of other months in month page. Default = false
  hideExtraDays={true}
  // If hideArrows=false and hideExtraDays=false do not switch month when tapping on greyed out
  // day from another month that is visible in calendar page. Default = false
  disableMonthChange={true}
  // If firstDay=1 week starts from Monday. Note that dayNames and dayNamesShort should still start from Sunday.
  firstDay={1}
  // Hide day names. Default = false
  hideDayNames={true}
  // Show week numbers to the left. Default = false
  showWeekNumbers={true}
  // Handler which gets executed when press arrow icon left. It receive a callback can go back month
  onPressArrowLeft={substractMonth => substractMonth()}
  // Handler which gets executed when press arrow icon left. It receive a callback can go next month
  onPressArrowRight={addMonth => addMonth()}
/>
```

#### Date marking

**!Disclaimer!** Make sure that `markedDates` param is immutable. If you change `markedDates` object content but the reference to it does not change calendar update will not be triggered.

Dot marking

<kbd>
  <img height=50 src="https://github.com/rghorbani/react-native-general-calendars/blob/master/demo/marking1.png?raw=true">
</kbd>

```javascript
<Calendar
  // Collection of dates that have to be marked. Default = {}
  markedDates={{
    '2012-05-16': {selected: true, marked: true, selectedColor: 'blue'},
    '2012-05-17': {marked: true},
    '2012-05-18': {marked: true, dotColor: 'red', activeOpacity: 0},
    '2012-05-19': {disabled: true, disableTouchEvent: true}
  }}
/>
```

You can customise a dot color for each day independently.

Multi-Dot marking

<kbd>
 <img height=50 src="https://github.com/rghorbani/react-native-general-calendars/blob/master/demo/marking4.png?raw=true">
</kbd>

Use markingType = 'multi-dot' if you want to display more than one dot. Both the Calendar and CalendarList control support multiple dots by using 'dots' array in markedDates. The property 'color' is mandatory while 'key' and 'selectedColor' are optional. If key is omitted then the array index is used as key. If selectedColor is omitted then 'color' will be used for selected dates.
```javascript
const vacation = {key:'vacation', color: 'red', selectedDotColor: 'blue'};
const massage = {key:'massage', color: 'blue', selectedDotColor: 'blue'};
const workout = {key:'workout', color: 'green'};

<Calendar
  markedDates={{
    '2017-10-25': {dots: [vacation, massage, workout], selected: true, selectedColor: 'red'},
    '2017-10-26': {dots: [massage, workout], disabled: true},
  }},
  markingType={'multi-dot'}
/>
```


Period marking

<kbd>
  <img height=50 src="https://github.com/rghorbani/react-native-general-calendars/blob/master/demo/marking2.png?raw=true">
</kbd>

<kbd>
  <img height=50 src="https://github.com/rghorbani/react-native-general-calendars/blob/master/demo/marking3.png?raw=true">
</kbd>

```javascript
<Calendar
  // Collection of dates that have to be colored in a special way. Default = {}
   markedDates={
    {'2012-05-20': {textColor: 'green'},
     '2012-05-22': {startingDay: true, color: 'green'},
     '2012-05-23': {selected: true, endingDay: true, color: 'green', textColor: 'gray'},
     '2012-05-04': {disabled: true, startingDay: true, color: 'green', endingDay: true}
    }}
  // Date marking style [simple/period/multi-dot]. Default = 'simple'
  markingType={'period'}
/>
```

Keep in mind that different marking types are not compatible. You can use just one marking style for calendar.

#### Displaying data loading indicator

<kbd>
  <img height=50 src="https://github.com/rghorbani/react-native-general-calendars/blob/master/demo/loader.png?raw=true">
</kbd>

The loading indicator next to month name will be displayed if `<Calendar />` has `displayLoadingIndicator` property and `markedDays` collection does not have a value for every day of the month in question. When you load data for days, just set `[]` or special marking value to all days in `markedDates` collection.

#### Customizing look & feel

```javascript
<Calendar
  // Specify style for calendar container element. Default = {}
  style={{
    borderWidth: 1,
    borderColor: 'gray',
    height: 350
  }}
  // Specify theme properties to override specific styles for calendar parts. Default = {}
  theme={{
    backgroundColor: '#ffffff',
    calendarBackground: '#ffffff',
    textSectionTitleColor: '#b6c1cd',
    selectedDayBackgroundColor: '#00adf5',
    selectedDayTextColor: '#ffffff',
    todayTextColor: '#00adf5',
    dayTextColor: '#2d4150',
    textDisabledColor: '#d9e1e8',
    dotColor: '#00adf5',
    selectedDotColor: '#ffffff',
    arrowColor: 'orange',
    monthTextColor: 'blue',
    textDayFontFamily: 'monospace',
    textMonthFontFamily: 'monospace',
    textDayHeaderFontFamily: 'monospace',
    textDayFontSize: 16,
    textMonthFontSize: 16,
    textDayHeaderFontSize: 16
  }}
/>
```

#### Advanced styling

If you want to have complete control over calendar styles you can do it by overriding default style.js files. For example, if you want to override calendar header style first you have to find stylesheet id for this file:

https://github.com/rghorbani/react-native-general-calendars/blob/master/src/calendar/header/style.js#L4

In this case it is 'stylesheet.calendar.header'. Next you can add overriding stylesheet to your theme with this id.

https://github.com/rghorbani/react-native-general-calendars/blob/master/example/src/screens/calendars.js#L56

```javascript
theme={{
  arrowColor: 'white',
  'stylesheet.calendar.header': {
    week: {
      marginTop: 5,
      flexDirection: 'row',
      justifyContent: 'space-between'
    }
  }
}}
```

**Disclaimer**: issues that arise because something breaks after using stylesheet override will not be supported. Use this option at your own risk.

#### Overriding day component

If you need custom functionality not supported by current day component implementations you can pass your own custom day
component to the calendar.

```javascript
<Calendar
  style={[styles.calendar, {height: 300}]}
  dayComponent={({date, state}) => {
    return (<View style={{flex: 1}}><Text style={{textAlign: 'center', color: state === 'disabled' ? 'gray' : 'black'}}>{date.day}</Text></View>);
  }}
/>
```

The dayComponent prop has to receive a RN component or function that receive props. The day component will receive such props:

* state - disabled if the day should be disabled (this is decided by base calendar component)
* marking - markedDates value for this day
* date - the date object representing this day

**Tip:** Don't forget to implement shouldComponentUpdate for your custom day component to make calendar perform better

If you implement an awesome day component please make a PR so that other people could use it :)

### CalendarList

<kbd>
  <img src="https://github.com/rghorbani/react-native-general-calendars/blob/master/demo/calendar-list.gif?raw=true">
</kbd>

`<CalendarList />` is scrollable semi-infinite calendar composed of `<Calendar />` components. Currently it is possible to scroll 4 years back and 4 years to the future. All paramters that are available for `<Calendar />` are also available for this component. There are also some additional params that can be used:

```javascript
<CalendarList
  // Callback which gets executed when visible months change in scroll view. Default = undefined
  onVisibleMonthsChange={(months) => {console.log('now these months are visible', months);}}
  // Max amount of months allowed to scroll to the past. Default = 50
  pastScrollRange={50}
  // Max amount of months allowed to scroll to the future. Default = 50
  futureScrollRange={50}
  // Enable or disable scrolling of calendar list
  scrollEnabled={true}
  // Enable or disable vertical scroll indicator. Default = false
  showScrollIndicator={true}
  ...calendarParams
/>
```

### Agenda

Coming soon

## Authors

* [Reza Ghorbani](https://github.com/rghorbani/) - Initial code - [@rghorbanif](https://twitter.com/RGhorbaniF)

See also the list of [contributors](https://github.com/rghorbani/react-native-general-calendars/contributors) who participated in this project.

## Contributing

Pull requests are welcome. `npm run test` and `npm run lint` before push.

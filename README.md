# vue-calendar

> A simple calendar with datepicker for vue.

## How to Use
1.引入Calendar.vue组件   

2.在使用该组件的页面配置
```
<input type="text" readonly @click="showCalendar($event,'picker1')" v-model="calendar.items.picker1.value" />
<input type="text" readonly @click="showCalendar($event,'picker2')" v-model="calendar.items.picker2.value" />
<input type="text" readonly @click="showCalendar($event,'picker3')" v-model="calendar.items.picker3.value" />
```
```
<calendar
:show.sync="calendar.show"
:x="calendar.x"
:y="calendar.y"
:value.sync="calendar.value"></calendar>

```
```
components: {
  Calendar
},
data() {
  return {
    calendar: {
      show: false,
      x: 0,
      y: 0,
      value: '',
      picker: '',
      items: {
        picker1: {
          value: ""
        },
        picker2: {
          value: ""
        },
        picker3: {
          value: ""
        }
      }
    }
  }
},
watch:{
  'calendar.value': function (value) {
    this.calendar.items[this.calendar.picker].value = value
  }
},
methods: {
  showCalendar: function(event,pickerName) {
    this.calendar.show = true
    this.calendar.x = event.target.offsetLeft
    this.calendar.y = event.target.offsetTop + event.target.offsetHeight + 7

    this.calendar.picker = pickerName
    this.calendar.value = this.calendar.items[pickerName].value
  }
}
```

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# run unit tests
npm run unit

# run e2e tests
npm run e2e

# run all tests
npm test
```

For detailed explanation on how things work, checkout the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).

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
:start-year="1926"
:end-year="2048"
:show.sync="calendar.show"
:x="calendar.x"
:y="calendar.y"
:choosed-start="calendar.choosedStart.value"
:choosed-end="calendar.choosedEnd.value"
:value.sync="calendar.value"
@click="getChoosedStart"></calendar>

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
      value: "",
      picker: "",
      choosedStart: {
        value: "",
        1: {
          value: ""
        }
      },
      choosedEnd: {
        value: "",
        1: {
          value: ""
        }
      },
      items: {
        // 单选
        picker1: {
          value: ""
        },
        // 区间
        picker2: {
          value: "",
          group: 1,
          type: "chooseStart"
        },
        picker3: {
          value: "",
          group: 1,
          type: "chooseEnd"
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
    this.calendar.choosedStart.value = ""
    this.calendar.choosedEnd.value = ""
    this.calendar.value = this.calendar.items[pickerName].value
    if (this.calendar.items[pickerName].type === "chooseEnd") {
      this.calendar.choosedStart.value = this.calendar.choosedStart[this.calendar.items[pickerName].group].value
    }
    else if (this.calendar.items[pickerName].type === "chooseStart") {
      this.calendar.choosedEnd.value = this.calendar.choosedEnd[this.calendar.items[pickerName].group].value
    }
  },
  getChoosedStart: function() {
    if (this.calendar.items[this.calendar.picker].type === "chooseStart") {
      this.calendar.items[this.calendar.picker].value = this.calendar.value
      this.calendar.choosedStart[this.calendar.items[this.calendar.picker].group].value = this.calendar.value
    }
    else if (this.calendar.items[this.calendar.picker].type === "chooseEnd") {
      this.calendar.items[this.calendar.picker].value = this.calendar.value
      this.calendar.choosedEnd[this.calendar.items[this.calendar.picker].group].value = this.calendar.value
    }
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

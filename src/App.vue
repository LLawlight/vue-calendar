<template>
  <div id="app">
    <img class="logo" src="./assets/logo.png">
    <div class="inputDate">
      <h2>单选模式第一组</h2>
      <input type="text" readonly @click="showCalendar($event,'picker1')" v-model="calendar.items.picker1.value" />
    </div>
    <div class="inputDate">
      <h2>单选模式第二组</h2>
      <input type="text" readonly @click="showCalendar($event,'picker6')" v-model="calendar.items.picker6.value" />
    </div>
    <div class="inputDate">
      <h2>区间模式第一组</h2>
      起始时间：
      <input type="text" readonly @click="showCalendar($event,'picker2')" v-model="calendar.items.picker2.value" />
      结束时间：
      <input type="text" readonly @click="showCalendar($event,'picker3')" v-model="calendar.items.picker3.value" />
    </div>
    <div class="inputDate">
      <h2>区间模式第二组</h2>
      起始时间：
      <input type="text" readonly @click="showCalendar($event,'picker4')" v-model="calendar.items.picker4.value" />
      结束时间：
      <input type="text" readonly @click="showCalendar($event,'picker5')" v-model="calendar.items.picker5.value" />
    </div>
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
    <p>
      Welcome to your Vue.js app!
    </p>
    <p>
      To get a better understanding of how this boilerplate works, check out
      <a href="http://vuejs-templates.github.io/webpack" target="_blank">its documentation</a>.
      It is also recommended to go through the docs for
      <a href="http://webpack.github.io/" target="_blank">Webpack</a> and
      <a href="http://vuejs.github.io/vue-loader/" target="_blank">vue-loader</a>.
      If you have any issues with the setup, please file an issue at this boilerplate's
      <a href="https://github.com/vuejs-templates/webpack" target="_blank">repository</a>.
    </p>
    <p>
      You may also want to checkout
      <a href="https://github.com/vuejs/vue-router/" target="_blank">vue-router</a> for routing and
      <a href="https://github.com/vuejs/vuex/" target="_blank">vuex</a> for state management.
    </p>
  </div>
</template>

<script>
import Calendar from './components/Calendar'

export default {
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
          },
          2: {
            value: ""
          }
        },
        choosedEnd: {
          value: "",
          1: {
            value: ""
          },
          2: {
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
          },
          picker4: {
            value: "",
            group: 2,
            type: "chooseStart"
          },
          picker5: {
            value: "",
            group: 2,
            type: "chooseEnd"
          },
          // 单选
          picker6: {
            value: ""
          }
        }
      }
    }
  },
  watch: {
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
}
</script>

<style>
html {
  height: 100%;
}

body {
  margin: 0;
  height: 100%;
}

h2 {
  color: #42b983;
}

#app {
  color: #2c3e50;
  max-width: 600px;
  font-family: Source Sans Pro, Helvetica, sans-serif;
  text-align: center;
  margin: 0 auto;
}

#app a {
  color: #42b983;
  text-decoration: none;
}

.logo {
  width: 100px;
  height: 100px
}

.inputDate {
  margin: 10px;
}
</style>

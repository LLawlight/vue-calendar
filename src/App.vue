<template>
  <div id="app">
    <img class="logo" src="./assets/logo.png">
    <hello></hello>
    <div class="inputDate">
      <input type="text" readonly @click="showCalendar($event,'picker1')" v-model="calendar.items.picker1.value" />
    </div>
    <div class="inputDate">
      <input type="text" readonly @click="showCalendar($event,'picker2')" v-model="calendar.items.picker2.value" />
      <input type="text" readonly @click="showCalendar($event,'picker3')" v-model="calendar.items.picker3.value" />
    </div>
    <calendar
    :show.sync="calendar.show"
    :x="calendar.x"
    :y="calendar.y"
    :value.sync="calendar.value"></calendar>
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
import Hello from './components/Hello'
import Calendar from './components/Calendar'

export default {
  components: {
    Hello,
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
          // 单选
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

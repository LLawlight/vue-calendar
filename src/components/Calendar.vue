<template>
  <div class="calendarContainer" v-show="show" :style="{ left: x + 'px' ,top: y + 'px'}">
    <div>
      <button type="button" name="preMonth" class="btn" @click="preMonth">&lt;</button>
      <select v-model="year">
        <option v-for="year in showYears">{{year+startYear}}</option>
      </select>
      年
      <select v-model="month">
        <option v-for="month in months">{{month}}</option>
      </select>
      月
      <button type="button" name="nextMonth" class="btn" @click="nextMonth">&gt;</button>
    </div>
    <table>
      <thead>
        <tr>
          <th v-for="day in days">{{day}}</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td v-for="td in firstTr" :class="{'disabled': ((td > 21) || (isDisabledStart(td) || isDisabledEnd(td))), 'active': (isActive(td) && !((td > 21) || (isDisabledStart(td) || isDisabledEnd(td)))), 'nowDate': isNowDate(td)}" @click="getDate">{{td}}</td>
        </tr>
        <tr v-for="tr in trs">
          <td v-for="td in tr" :class="{'disabled': (isDisabledStart(td) || isDisabledEnd(td)), 'active': (isActive(td) && !(isDisabledStart(td) || isDisabledEnd(td))), 'nowDate': isNowDate(td)}" @click="getDate">{{td}}</td>
        </tr>
        <tr>
          <td v-for="td in lastTr" :class="{'disabled': ((td < 8) || (isDisabledStart(td) || isDisabledEnd(td))), 'active': (isActive(td) && !(isDisabledStart(td) || isDisabledEnd(td))), 'nowDate': isNowDate(td)}" @click="getDate">{{td}}</td>
        </tr>
      </tbody>
    </table>
    <button type="button" name="today" class="btn" @click="today">返回今天</button>
    <button type="button" name="close" class="btn" @click="close">关闭日历</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      nowYear: 0,
      nowMonth: 0,
      nowDate: 0,
      choosedStartYear: 0,
      choosedStartMonth: 0,
      choosedStartDate: 0,
      choosedEndYear: 0,
      choosedEndMonth: 0,
      choosedEndDate: 0,
      firstTr: [],
      trs: [],
      lastTr: [],
      months: [1,2,3,4,5,6,7,8,9,10,11,12],
      days: ['日','一','二','三','四','五','六']
    }
  },
  props: {
    startYear: {
      type: Number,
      default: 1900
    },
    endYear: {
      type: Number,
      default: 2099
    },
    year: {
      type: Number,
      default: 1993
    },
    month: {
      type: Number,
      default: 6
    },
    show: {
      type: Boolean,
      twoWay: true,
      default: false
    },
    value: {
      type: String,
      twoWay: true,
      default: ""
    },
    x: {
      type: Number,
      default: 0
    },
    y: {
      type: Number,
      default: 0
    },
    choosedStart: {
      type: String,
      default: ""
    },
    choosedEnd: {
      type: String,
      default: ""
    }
  },
  computed: {
    showYears: function() {
      return this.endYear - this.startYear + 1
    },
    // 当前月第一天是星期几
    startDay: function() {
      return new Date(this.year, this.month - 1, 1).getDay()
    },
    // 当前月的下个月的第一天是星期几
    nextStartDay: function() {
      return new Date(this.year, this.month, 1).getDay()
    },
    addDatesEnd: function() {
      return 6 - new Date(this.year, this.month, 0).getDay()
    },
    // 当前月最后一天是几号
    endDate: function() {
      return new Date(this.year, this.month, 0).getDate()
    },
    // 当前月的上个月的最后一天是几号
    lastEndDate: function() {
      return new Date(this.year, this.month - 1, 0).getDate()
    }
  },
  ready() {
    this.init()
    this.creatTbody()
  },
  attached() {},
  watch: {
    'year': function() {
      this.creatTbody()
    },
    'month': function() {
      this.creatTbody()
    }
  },
  methods: {
    init: function() {
      var now = new Date()
      this.nowYear = now.getFullYear()
      this.nowMonth = now.getMonth() + 1
      this.nowDate = now.getDate()
      this.year = this.nowYear
      this.month = this.nowMonth
    },
    creatTbody: function() {
      this.firstTr = []
      this.trs = [];
      this.lastTr = []

      for (var i = 0; i < this.startDay; i++) {
        this.firstTr.push( this.lastEndDate - this.startDay + i + 1 )
      }
      for (var i = 1; i <= 7 - this.startDay; i++) {
        this.firstTr.push(i)
        this.dateStart = i + 1
      }

      for (var i = 0; i < this.nextStartDay; i++) {
        this.lastTr.push( this.endDate - this.nextStartDay + i + 1 )
      }
      if (this.lastTr[0]) {
        for (var i = 1; i <= 7 - this.nextStartDay; i++) {
          this.lastTr.push(i)
        }
      }
      this.dateEnd = this.endDate - this.nextStartDay

      var dateNum = this.dateStart
      for (var i = 0; i < (this.dateEnd - this.dateStart + 1) / 7; i++) {
        this.trs[i]=[]
        for (var j = 0; j < 7; j++) {
          this.trs[i].push(dateNum++)
        }
      }
    },
    preMonth: function(){
      if (this.month == this.months[0]) {
        this.year--
        this.month = this.months[11]
      }
      else {
        this.month--
      }
    },
    nextMonth: function() {
      if (this.month == this.months[11]) {
        this.year++
        this.month = this.months[0]
      }
      else {
        this.month++
      }
    },
    today: function() {
      this.year = this.nowYear
      this.month = this.nowMonth
    },
    close: function() {
      this.show = false
      this.year = this.nowYear
      this.month = this.nowMonth
    },
    getDate: function(event) {
      if (event.target.className.indexOf('disabled') === -1) {
        var thisMonth = this.month,
            thisDate = event.target.innerText;
        thisMonth = (thisMonth < 10) ? ('0' + thisMonth) : thisMonth
        thisDate = (thisDate < 10)?('0' + thisDate):thisDate
        this.value = this.year + '.' + thisMonth + '.' + thisDate
        this.close()
      }
    },
    isNowDate: function(td) {
      return ((this.year == this.nowYear) && (this.month == this.nowMonth) && (td == this.nowDate))
    },
    isActive: function(td) {
      return (this.year == this.value.split('.')[0]) && (this.month == this.value.split('.')[1]) && (td == this.value.split('.')[2])
    },
    isDisabledStart: function(td) {
      this.choosedStartYear = this.choosedStart.split('.')[0]
      this.choosedStartMonth = Number(this.choosedStart.split('.')[1])
      this.choosedStartDate = this.choosedStart.split('.')[2]
      return ((this.choosedStartYear > this.year) || ((this.choosedStartYear == this.year) && (this.choosedStartMonth > this.month)) ||((this.choosedStartYear == this.year) && (this.choosedStartMonth == this.month) && (this.choosedStartDate > td)))
    },
    isDisabledEnd: function(td) {
      this.choosedEndYear = this.choosedEnd.split('.')[0]
      this.choosedEndMonth = Number(this.choosedEnd.split('.')[1])
      this.choosedEndDate = this.choosedEnd.split('.')[2]
      if (this.choosedEndYear!=="") {
        return ((this.choosedEndYear < this.year) || ((this.choosedEndYear == this.year) && (this.choosedEndMonth < this.month)) ||((this.choosedEndYear == this.year) && (this.choosedEndMonth == this.month) && (this.choosedEndDate < td)))
      }
      else {
        return false
      }
    }
  },
  components: {}
};
</script>

<style scoped>
.calendarContainer {
  position: absolute;;
  display: inline-block;
  border: 1px solid rgba(0,0,0,0.2);
  border-radius: 4px;
  background-color: #fff;
  padding: 10px;
  color: #333;
  -webkit-user-select:none;
  -moz-user-select:none;
  -o-user-select:none;
  user-select:none;
}

.calendarContainer:before {
  content: '';
  display: inline-block;
  border-left: 7px solid transparent;
  border-right: 7px solid transparent;
  border-bottom: 7px solid #ccc;
  border-bottom-color: rgba(0, 0, 0, 0.2);
  position: absolute;
  top: -7px;
  left: 6px;
}

.calendarContainer:after {
  content: '';
  display: inline-block;
  border-left: 6px solid transparent;
  border-right: 6px solid transparent;
  border-bottom: 6px solid #ffffff;
  position: absolute;
  top: -6px;
  left: 7px;
}

table {
  margin: 0 auto;
}

tbody td:hover {
  background-color: #eeeeee;
  cursor: pointer;
}

td {
  width: 20px;
  height: 2px;
  border-radius: 4px;
  padding: 4px 5px;
}

.disabled {
  color: #bfbfbf;
}

.active {
  background-color: #42b983;
}

.nowDate{
  background-color: #0044cc;
  background-image: -webkit-gradient(linear, 0 0, 0 100%, from(#0088cc), to(#0044cc));
  background-image: -webkit-linear-gradient(top, #0088cc, #0044cc);
  color: #fff;
}
.btn {
  border: 1px solid #999;
  background: #fafafa;
  color: #7a7a7a;
  font-weight: 700;
  font-family: Simsun,Simhei,sans-serif;
}

.btn:active {
  border-color: #a2a6ab;
  background-color: #f0f0f0;
  box-shadow: inset 1px 1px 1px #c7c7c7;
  -webkit-box-shadow: inset 1px 1px 1px #c7c7c7;
  -moz-box-shadow: inset 1px 1px 1px #c7c7c7;
  -o-box-shadow: inset 1px 1px 1px #c7c7c7;
}
</style>

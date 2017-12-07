<template>
  <div class="white">
    <div class="flex">
      <select v-model="year" @change="resetDate">
        <option v-for="item in 2050" :key="item" v-if="item >= 1900 && item <= 2050">
          {{item}}
        </option>
      </select>
      <select v-model="month" @change="resetDate">
        <option v-for="item in 12" :key="item">
          {{item}}
        </option>
      </select>
      {{date > 0 ? year + '/' + month + '/' + date : ''}}
      <div class="spacer"></div>
      <button @click="prevMonth">上一月</button>
      <button @click="nextMonth">下一月</button>
      <button @click="year = year + 1">上一年</button>
      <button @click="year = year - 1">下一年</button>
      <div class="spacer"></div>
    </div>
    <div class="date-content">
      <table border="0" cellpadding="0" cellspacing="0" class="table">
        <thead>
          <tr>
            <th v-for="item in week" :key="item">{{item}}</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in dayArr" :key="item[0]">
            <td v-for="day in item" :key="day"
              :class="{'active': date === day, 'nowdate': day === nowDate && year === nowFullYear && month === nowMonth}"
              @click="dayClick(day)"
            >
              {{day}}
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      nowFullYear: '',
      nowMonth: '',
      nowDate: '',
      year: '',
      month: '',
      date: 0,
      week: ['周一', '周二', '周三', '周四', '周五', '周六', '周日'],
      isActive: false
    }
  },
  mounted () {
    let date = new Date()
    this.nowFullYear = date.getFullYear()
    this.nowMonth = date.getMonth() + 1
    this.nowDate = date.getDate()
    this.year = date.getFullYear()
    this.month = date.getMonth() + 1
    this.date = date.getDate()
  },
  methods: {
    resetDate () {
      this.date = 0
    },
    isloop () {
      return this.year % 400 === 0 ? 1 : this.year % 4 === 0 ? 1 : 0
    },
    dayClick (day) {
      this.date = day
    },
    prevMonth () {
      if (this.month > 1) {
        this.month = this.month - 1
      } else {
        this.month = 12
        this.year = this.year - 1
      }
    },
    nextMonth () {
      if (this.month < 12) {
        this.month = this.month + 1
      } else {
        this.month = 1
        this.year = this.year + 1
      }
    }
  },
  computed: {
    monthDays () {
      return [31, 28 + this.isloop(), 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]
    },
    fristDayWeek () {
      return new Date(this.year + '-' + this.month + '-' + 1).getDay()
    },
    selfMonthDay () {
      return this.monthDays[this.month - 1]
    },
    tdLength () {
      return this.selfMonthDay + this.fristDayWeek - 1
    },
    trLength () {
      return this.tdLength / 7
    },
    dayArr () {
      let arr = []
      let k = 0
      for (let i = 0; i < this.trLength; i++) {
        let monthArr = []
        for (let j = 0; j < 7; j++) {
          if (i === 0) {
            if (j < this.fristDayWeek - 1) {
              monthArr.push('')
            } else {
              k++
              monthArr.push(k)
            }
          } else {
            k++
            if (k <= this.selfMonthDay) {
              monthArr.push(k)
            } else {
              monthArr.push('')
            }
          }
        }
        arr.push(monthArr)
      }
      return arr
    }
  }
}
</script>

<style>
  button + button,
  select + select {
    margin-left: 10px;
  }
  .white{
    background:#fff;
  }
  .flex{
    display: flex;
    padding: 15px;
  }
  .spacer{
    flex: 1;
  }
  .date-content{
    padding: 15px;
  }
  .table {
      border-left: solid 1px #ccc;
      border-top: solid 1px #ccc;
  }

  .table th,
  .table td {
      border-right: solid 1px #ccc;
      border-bottom: solid 1px #ccc;
      padding: 5px 10px;
  }

  .table thead {
      background: #e1e1e1;
  }

  .table tbody tr:nth-child(even) {
      background: #f3f3f3;
  }
  .table td{
    cursor: pointer;
  }
  .active{
    background: black;
    color: white;
  }
  .nowdate{
    background: red;
    color: white;
  }
</style>

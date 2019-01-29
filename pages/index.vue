<template>
  <div>
    <div class="main__wrapper">
      <div class="calendar__wrapper">
        <div class="date__warpper">
          <div class="btn-nav btn-prev" @click="prevMonth"></div>
          <div>{{monthNow}} {{year}}</div>
          <div class="btn-nav btn-next" @click="nextMonth"></div>
        </div>
        <div class="datapicker__wrapper">
          <div v-for="(test, i) in dayName"
               :key="i"
               class="date">
            <div>{{test}}</div>
          </div>
          <div v-for="(date, i) in monthDays"
               :key="i" class="date"
               :class="date.currentDay ? 'current-day':'' || date.currentMonth ? '' : 'current-month'"
          >
            <div>{{date.day}}</div>
          </div>
        </div>
      </div>
    </div>
    <div class="input__wrapper">
      <input type="text" v-model="changeDay" @keyup.enter="searchDay" placeholder="ДД.ММ.ГГГГ">
    </div>
  </div>
</template>

<script>


export default {
  data() {
    return {
      date: new Date(),
      monthName: ['Январь', 'Февраль', 'Март', 'Апрель', 'Май', 'Июнь', 'Июль', 'Август', 'Сентябрь', 'Октябрь',
        'Ноябрь', 'Декабрь'],
      dayName: ['пн', 'вт', 'ср ', 'чт', 'пт', 'сб', 'вс'],
      monthNow: '',
      monthCount: 1,
      monthDays: [],
      year: new Date().getFullYear(),
      month: new Date().getMonth(),
      currentDay: {
        day:  new Date().getDate(),
        month: new Date().getMonth(),
        year: new Date().getFullYear()
      },
      changeDay: ''
    }
  },
  methods: {
    searchDay() {
      if (!this.changeDay) {
        return
      }
      let parseDate = this.changeDay.split('.')
      let month = +parseDate[1] - 1
      this.currentMonth(parseDate[2], month, parseDate[0])
      this.month =  month
      this.year = parseDate[2]
    },
    prevMonth() {
      if (this.month == 0) {
        this.month = 11
        this.year--
      } else {
        this.month = this.month - 1
      }
      this.currentMonth(this.year, this.month)
    },
    nextMonth() {
      if (this.month == 11) {
        this.month = 0
        this.year++
      } else {
        this.month = this.month + 1
      }
      this.currentMonth(this.year, this.month)
    },
    currentMonth(year, month, searchDay) {
      this.monthDays = []
      this.monthNow = ''
      this.monthNow = this.monthName[month]
      let daysInMonth = 33 - new Date(year, month, 33).getDate()

      let firstDayInMonth = new Date(year, month, 1).getDay()
      let lastDayInMonth = new Date(year, month, daysInMonth).getDay()
      if (firstDayInMonth == 0) {
        firstDayInMonth = 7
      }
      if (firstDayInMonth != 1) {
        let daysInPrevMonth = 33 - new Date(year, month - 1, 33).getDate()
        for (let i = daysInPrevMonth - (firstDayInMonth - 2); i < daysInPrevMonth + 1; i++ ) {
          this.monthDays.push({day: i, currentDay: false, currentMonth: false});
        }
      }

      for (let i = 1; i < daysInMonth + 1; i ++) {
        let date = new Date(year, month, i)
        if (date.getDate() === this.currentDay.day && month === this.currentDay.month && year ===
          this.currentDay.year) {
          this.monthDays.push({day: date.getDate(), currentDay: true, currentMonth: true})
        }else if (searchDay == date.getDate()) {
          this.monthDays.push({day: date.getDate(), currentDay: true, currentMonth: true})
        }else {
          this.monthDays.push({day: date.getDate(), currentDay: false, currentMonth: true})
        }
      }

      if (lastDayInMonth != 0) {
        for (let i = 1; i <= 7 -  lastDayInMonth; i++) {
          this.monthDays.push({day: i, currentDay: false, currentMonth: false});
        }
      }
      for (let i = 0; i < 7; i++) {
        this.monthDays[i].weekDay = this.dayName[i]
      }
    }
  },
  mounted() {
   this.currentMonth(this.year, this.month)
  }
}
</script>

<style lang="scss">
  .main__wrapper {
    display: flex;
    flex-direction: column;
  }
  .datapicker__wrapper {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: space-between;
    max-width: 370px;
    color:black;
  }
  .days-name {
    width: 50px;
    height: 50px;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .date {
    width: 50px;
    height: 50px;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }
  .current-day {
    background-color: red;
    color: white;
    border-radius: 50%;
  }
  .current-month {
    color: lightgrey;
  }
  .calendar__wrapper {
    display: flex;
    flex-direction: column;
    max-width: 370px;
    margin: 0 auto;
  }
  .date__warpper {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
  }
  .input__wrapper {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 25px;
    input {
      outline: none;
    }
  }
  .btn-nav {
    border: solid lightgrey;
    border-width: 0 .2em .2em 0;
    display: inline-block;
    padding: .30em;
    &:hover {
      border: solid black;
      border-width: 0 .2em .2em 0;
    }
  }
  .btn-prev {
    transform:rotate(135deg);
    margin-left: 15px;
  }
  .btn-next {
    transform:rotate(-45deg);
    margin-left: 15px;
  }

</style>


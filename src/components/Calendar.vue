<template>
  <div class="vue-infinite-calendar">
    <div class="calendar-header" :style="{width: width}">
      <a href="javascript:void(0)" class="today-button" @click="scrollToDate(new Date())">↪Today</a>
      <div class="current-year">{{currentYear}}</div>
      <div class="week-days">
        <div class="week-day" v-for="weekday in weekdays">{{weekday}}</div>
      </div>
    </div>
    <div class="calendar-container" :style="{width: width, height: height}" @scroll="handleScroll">
      <MonthList
        :months="months"
        :monthNames="monthNames"
        :firstDay="firstDay"
        :onSelect="onSelect"
        :onDeselect="onDeselect"
        :selectedDates="selectedDates"
      />
    </div>
  </div>
</template>

<script>
import MonthList from './monthList.vue'

export default {
  components: {
    MonthList
  },
  data () {
    return {
      currentYear: new Date().getFullYear(),
      selectedDates: []
    }
  },
  computed: {
    weekdays () {
      let weekDays = []
      let firstDay = this.firstDay
      while (weekDays.length !== 7) {
        weekDays.push(this.dayNames[firstDay])
        firstDay += 1
        if (firstDay >= this.dayNames.length) { firstDay = 0 }
      }
      return weekDays
    },
    months () {
      let month = this.min.getMonth()
      let year = this.min.getFullYear()
      let currentDate = this.min
      let displayedMonths = []

      while (currentDate.getTime() < this.max.getTime()) {
        displayedMonths.push({ year: year, month: month })
        month += 1
        if (month >= 12) {
          year += 1
          month -= 12
        }
        currentDate = new Date(year, month)
      }
      return displayedMonths
    },
    headerHeight () {
      return this.$el.querySelector('.calendar-header').getBoundingClientRect().height
    }
  },
  created () {
    this.selected.forEach((date) => {
      const isoDate = `${date.getFullYear()}-${date.getMonth() + 1}-${date.getDate()}`
      this.selectedDates.push(isoDate)
    })
  },
  mounted () {
    this.scrollToDate(new Date())
  },
  props: {
    selected: {
      type: Array,
      default () { return [] }
    },
    min: {
      type: Date,
      default () {
        return new Date(1980, 0, 1)
      }
    },
    max: {
      type: Date,
      default () {
        return new Date(2050, 11, 31)
      }
    },
    width: {
      type: String,
      default: '300px'
    },
    height: {
      type: String,
      default: '500px'
    },
    monthNames: {
      type: Array,
      default () {
        return ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December']
      }
    },
    dayNames: {
      type: Array,
      default () { return ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'] }
    },
    firstDay: {
      type: Number,
      default: 1 // Monday
    },
    onSelect: {
      type: Function,
      default () { }
    },
    onDeselect: {
      type: Function,
      default () { }
    }
  },
  methods: {
    scrollToDate (date) {
      const month = date.getMonth()
      const year = date.getFullYear()
      this.scrollToMonth(year, month)
    },
    scrollToMonth (year, month) {
      const headerHeight = this.headerHeight
      const offset = this.$el.querySelector(`.month[data-month="${month}"][data-year="${year}"]`).offsetTop
      this.$el.querySelector('.calendar-container').scrollTop = offset - headerHeight
    },
    handleScroll (e) {
      const headerHeight = this.headerHeight
      const scrollTop = this.$el.querySelector('.calendar-container').scrollTop
      const years = this.$el.querySelectorAll('.year')
      for (let i = 0; i < years.length; i++) {
        const year = years[i]
        if (scrollTop <= year.offsetTop - (headerHeight - 10)) {
          if (i !== 0) { this.currentYear = Number(year.innerHTML) - 1 }
          return
        }
      }
      this.currentYear = this.max.getFullYear()
    }
  }
}
</script>

<style lang="scss">
.vue-infinite-calendar {
  position: relative;
}

.calendar-container {
  overflow: scroll;
  margin: 0 auto;
  -webkit-overflow-scrolling: touch;
  box-shadow: inset 0px 10px 10px -10px #CCC, inset 0px -10px 10px -10px #CCC;
}

.calendar-header {
  background-color: white;
  width: inherit;
  margin: 0 auto;

  .current-year {
    font-size: 1.5rem;
  }

  .today-button {
    float: right;
    text-decoration: none;
  }

  .week-days {
    width: 100%;
    margin-bottom: 5px;

    .week-day {
      display: inline-block;
      width: 14.28571%;
      text-align: center;
    }
  }
}
</style>

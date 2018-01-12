<template>
  <div>
    <Calendar :onSelect="toggleDay" :onDeselect="toggleDay" />
    <hr/>
    <div class="selection">
      <b>Selected:</b> <br/>
      {{selectedDates}}
    </div>
  </div>
</template>

<script>
import Calendar from './components/Calendar.vue'

export default {
  name: 'app',
  components: {
    Calendar
  },
  data () {
    return {
      selectedDates: []
    }
  },
  methods: {
    getISODate (date) {
      return `${date.getFullYear()}-${date.getMonth() + 1}-${date.getDate()}`
    },
    toggleDay (date) {
      const isoDate = this.getISODate(date)
      const index = this.selectedDates.indexOf(isoDate)
      if (index !== -1) {
        this.selectedDates.splice(index, 1)
      } else {
        this.selectedDates.push(isoDate)
      }
    }
  }
}
</script>

<style lang="scss">
*, *::before, *::after {
  box-sizing: border-box;
}

#app {
  margin: 0 auto;
  line-height: 1.4;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: blue;
}

h1 {
  text-align: center;
  font-size: 3rem;
}

.selection {
  margin-top: 25px;
  text-align: center;
}
</style>

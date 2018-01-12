<template>
  <div :class="{day: true, 'is-today': isToday, 'is-selected': selected }" @click="handleClick(day)">
    {{day.getDate()}}
  </div>
</template>

<script>
export default {
  methods: {
    handleClick (day) {
      this.selected = !this.selected
      if (this.selected) { this.onSelect(day) } else { this.onDeselect(day) }
    }
  },
  computed: {
    isToday () {
      const today = new Date()
      const day = this.day
      return day.getFullYear() === today.getFullYear() &&
             day.getMonth() === today.getMonth() &&
             day.getDate() === today.getDate()
    }
  },
  mounted () {
    this.selected = this.isSelected
  },
  data () {
    return {
      selected: false
    }
  },
  props: {
    day: {
      type: Date,
      required: true
    },
    onSelect: {
      type: Function,
      required: true
    },
    onDeselect: {
      type: Function,
      required: true
    },
    isSelected: {
      type: Boolean,
      required: true
    }
  }
}
</script>

<style>
.day {
  cursor: pointer;
  display: inline-block;
  height: 43px;
  line-height: 43px;
  text-align: center;
  border-radius: 22px;
  width: 14.28571%;
}

.day.is-today {
  background-color: blanchedalmond;
}

.day.is-selected {
  background-color: lightblue;
}

.day.is-selected:hover {
  background-color: lightskyblue;
}

.day:hover {
  background-color: whitesmoke;
}
</style>

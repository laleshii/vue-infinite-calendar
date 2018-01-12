<template>
  <div class="month-list">
    <template v-for="month in months">
      <div class="year" v-if="month.month === 0">{{month.year}}</div>
      <Month
        :month="month.month"
        :year="month.year"
        :monthNames="monthNames"
        :firstDay="firstDay"
        :onSelect="onSelect"
        :onDeselect="onDeselect"
        :selectedDates="selectedDates"
      />
    </template>
  </div>
</template>

<script>
import Month from './Month.vue'

export default {
  components: {
    Month
  },
  props: {
    months: {
      type: Array,
      required: true
    },
    monthNames: {
      type: Array,
      required: true
    },
    firstDay: {
      type: Number,
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
    selectedDates: {
      type: Array,
      required: true
    }
  }
}
</script>
<style lang="scss">
.month-list .year:first-child {
  display:none;
}

.year {
  margin-top: 15px;
  font-size: 1.5rem;
}
</style>

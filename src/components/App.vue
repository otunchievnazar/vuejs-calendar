<template>
  <div>
    <div v-for="week in weeks">
      <calendar-day v-for="day in weeks" :day="day"> {{ day }}</calendar-day>
    </div>
  </div>
</template>

<script>
import CalendarDay from './CalendarDay.vue'

export default {
  data () {
    return {
      month: 5,
      year: 2021,
    };
  },
  components: {
    CalendarDay
  },
  computed: {
    days () {
      let days = [];
      let currentDay = this.$moment(`${this.year}-${this.month}-1`, 'YYYY-M-D');
      do {
        days.push(currentDay);
        currentDay = this.$moment(currentDay).add(1, 'days');
      } while ((currentDay.month() + 1) === this.month);
      //add previous days to start
      currentDay = this.$moment(days[0]);
      const MONDAY = 1;
      const SUNDAY = 0;

      if (currentDay.day() !== MONDAY) {
        do {
          currentDay = this.$moment(currentDay).subtract(1, 'days');
          days.unshift(currentDay);
        } while (currentDay.day() !== MONDAY);
      }

      // add following days to end
      currentDay = this.$moment(days[days.length - 1]);

      if (currentDay.day() !== SUNDAY) {
        do {
          currentDay = this.$moment(currentDay).add(1, 'days');
          days.push(currentDay);
        } while (currentDay.day() !== SUNDAY);
      }

      return days;
    },
    weeks () {
      let weeks = [];
      let week = [];

      for (let day of this.days) {
        week.push(day);
        if (week.length === 7) {
          weeks.push(week);
          week = [];
        }
      }
      return weeks;
    },
  },
};
</script>

<style scoped>

</style>

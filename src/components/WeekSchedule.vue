<template>
  <v-container mt-0 pt-0 v-resize="onResize">
    <v-layout>
      <v-flex>
        <v-sheet height="400">
          <!-- now is normally calculated by itself, but to keep the calendar in this date range to view events -->
          <v-calendar
            id="calendar"
            light
            ref="calendar"
            :now="today"
            :value="today"
            color="primary"
            type="week"
            :weekdays="weekdays"
            :interval-count="8"
            :first-interval="10"
            :short-weekdays="shortWeekdays"
          >
            <!-- the events at the bottom (timed) -->
            <template v-slot:dayBody="{ date, timeToY, minutesToPixels }">
              <template v-for="event in eventsMap[date]">
                <div
                  v-if="event.time"
                  :key="event.title"
                  :style="{ top: timeToY(event.time)+1 + 'px', height: minutesToPixels(event.duration) + 'px' }"
                  class="my-event with-time"
                  @click="open(event)"
                  v-html="event.title"
                ></div>
              </template>
            </template>
          </v-calendar>
        </v-sheet>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
export default {
  props: {
    userData: {
      startTime: Number, //hour in which is the earliest class in week
      hoursDaily: Number, //maximum number of hours needed to show in table
      events: []
    }
  },
  data: () => ({
    today: "2019-05-17",
    weekdays: [1, 2, 3, 4, 5, 6, 0], // order of weekday names
    shortWeekdays: false //display short weekday names on smaller screens
  }),
  computed: {
    // convert the list of events into a map of lists keyed by date
    eventsMap() {
      console.log("map",this.userData)
      const map = {};
      this.userData.events.forEach(e =>
        (map[e.date] = map[e.date] || []).push(e)
      );
      return map;
    }
  },
  mounted() {
    this.$refs.calendar.scrollToTime("08:00");
    this.onResize();
  },
  methods: {
    open(event) {
      alert(event.title);
    },
    onResize() {
      if (window.innerWidth >= 1200) {
        this.shortWeekdays = false;
      } else {
        this.shortWeekdays = true;
      }
    }
  }
};
</script>

<style>
.v-calendar-daily_head-weekday {
  /* font-size: calc(7px + 1vw); */
  font-size: 13pt;
  color: black !important;
  height: 1vh !important;
}
.v-calendar-daily_head-day-label {
  visibility: hidden;
}
#calendar {
  overflow: hidden;
}
</style>

<style lang="stylus" scoped>
.my-event {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  border-radius: 2px;
  background-color: #1867c0;
  color: #ffffff;
  border: 1px solid #1867c0;
  font-size: 12px;
  padding: 3px;
  cursor: pointer;
  margin-bottom: 1px;
  left: 4px;
  margin-right: 8px;
  position: relative;

  &.with-time {
    position: absolute;
    right: 4px;
    margin-right: 0px;
  }
}
</style>
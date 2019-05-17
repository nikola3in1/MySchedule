<template>
  <v-app dark>
    <v-content>
      <v-layout align-center justify-center>
        <v-flex md8>
          <HelloWorld/>
          <div v-if="firstUse"></div>
          <div v-else>
            <WeekSchedule v-if="showWeek" :userData="userData"/>
            <CourseSchedule v-if="showCourse"/>
            <Navigation v-on:toggle="toggle"/>
          </div>
          <v-btn @click="saveData(userData)">save</v-btn>
          <v-btn @click="removeEvents">reset</v-btn>
          <v-btn @click="initSampleData">init</v-btn>
        </v-flex>
      </v-layout>
    </v-content>
  </v-app>
</template>

<script>
import HelloWorld from "./components/HelloWorld";
import WeekSchedule from "./components/WeekSchedule";
import CourseSchedule from "./components/CourseSchedule";
import Navigation from "./components/Navigation";

export default {
  name: "App",
  components: {
    HelloWorld,
    WeekSchedule,
    CourseSchedule,
    Navigation
  },
  data: () => ({
    //Menu
    showWeek: true,
    showCourse: false,
    showUpdate: false,

    //State
    firstUse: false,

    //Data
    days: {
      // values for easier weekday date access
      mon: "2019-05-13",
      tue: "2019-05-14",
      wed: "2019-05-15",
      thu: "2019-05-16",
      fri: "2019-05-17",
      sat: "2019-05-18",
      sun: "2019-05-19"
    },
    userData: {
      startTime: 0, //hour in which is the earliest class in week
      hoursDaily: 0, //maximum number of hours needed to show in table
      events: []
    }
  }),
  mounted() {
    let data = this.loadData();
    console.log("loaded data", data);
    if (data.events == null) {
      //First time here
      this.firstUse = true;
      console.log("first time here");
    } else {
      //Set loaded data
      this.userData = data;
    }

    //dummy data
    // this.userData.events = [
    //   {
    //     title: "Weeklawdy Meeting",
    //     date: this.days["mon"],
    //     time: "12:00",
    //     duration: 45
    //   },
    //   {
    //     title: "Weeklawdy Meeting",
    //     date: this.days["tue"],
    //     time: "12:00",
    //     duration: 45
    //   },
    //   {
    //     title: "Weeklawdy Meeting",
    //     date: this.days["fri"],
    //     time: "12:00",
    //     duration: 45
    //   },
    //   {
    //     title: "Mash Potatoes",
    //     date: "2019-01-09",
    //     time: "12:30",
    //     duration: 180
    //   }
    // ];
  },
  methods: {
    toggle: function(tab) {
      if (tab === "week") {
        console.log("week");
        this.showWeek = true;
        this.showCourse = false;
        this.showUpdate = false;
      } else if (tab === "course") {
        this.showCourse = true;
        this.showWeek = false;
        this.showUpdate = false;
      } else if (tab === "update") {
        this.showUpdate = true;
        this.showWeek = false;
        this.showCourse = false;
      }
    },
    saveData(userData) {
      document.cookie = JSON.stringify(userData);
      console.log(this.loadData());
    },
    loadData() {
      let data = JSON.parse(document.cookie);
      return data;
    },
    removeEvents() {
      this.userData.events = [];
      this.saveData({});
    },
    initSampleData() {
      this.userData = {
        startTime: 0, //hour in which is the earliest class in week
        hoursDaily: 0, //maximum number of hours needed to show in table
        events: [
          {
            title: "Weeklawdy Meeting",
            date: this.days["mon"],
            time: "12:00",
            duration: 45
          },
          {
            title: "Weeklawdy Meeting",
            date: this.days["tue"],
            time: "12:00",
            duration: 45
          },
          {
            title: "Weeklawdy Meeting",
            date: this.days["fri"],
            time: "12:00",
            duration: 45
          },
          {
            title: "Mash Potatoes",
            date: "2019-01-09",
            time: "12:30",
            duration: 180
          }
        ]
      };
      this.firstUse = false;
      console.log("after init", this.userData);
    }
  }
};
</script>
<style>
::-webkit-scrollbar {
  width: 0em;
  background: yellow;
  display: inline !important;
}

#app {
  overflow: hidden !important;
  background: #36d1dc !important; /* fallback for old browsers */
  background: -webkit-linear-gradient(
    to right,
    #5b86e5,
    #36d1dc
  ) !important; /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(
    to right,
    #5b86e5,
    #36d1dc
  ) !important; /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
}
</style>

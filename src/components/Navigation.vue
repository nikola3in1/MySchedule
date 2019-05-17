<template>
  <v-container pt-0>
    <v-tabs light grow fixed-tabs slider-color="blue">
      <v-tab v-on:click="week">Week</v-tab>
      <v-tab v-on:click="course">Course</v-tab>
      <v-tab v-on:click="update">Update</v-tab>
    </v-tabs>
  </v-container>
</template>
<script>
export default {
  data() {
    return {
    };
  },
  methods: {
    week: function() {
      this.$emit('toggle',"week");
    },
    course: function() {
      this.$emit('toggle',"course");
    },
    update: function() {
      this.$emit('toggle',"update");
    }
  }
};
</script>
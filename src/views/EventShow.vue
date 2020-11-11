<template>
  <div class="user">
    <h1>Showing event {{ id }} - {{ event.title }}</h1>
  </div>
</template>
<script>
import EventService from "@/services/EventService.js";
export default {
  props: ["id"],
  data() {
    return {
      event: {}
    };
  },
  created() {
    EventService.getEvent(this.id)
      .then(response => {
        this.event = response.data;
      })
      .catch(err => {
        console.log(`Error: ${err.response}`);
      });
  }
};
</script>

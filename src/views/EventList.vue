<template>
  <div>
    <h4>Events Listing</h4>
    <EventCard v-for="event in events" :key="event.id" :event="event" />
  </div>
</template>

<script>
import EventCard from "@/components/EventCard.vue";
import EventService from "@/services/EventService.js";

export default {
  components: {
    EventCard
  },
  data() {
    return {
      events: []
    };
  },
  created() {
    EventService.getEvents()
      .then(response => {
        this.events = response.data;
      })
      .catch(error => {
        console.log(`Error: ${error.response}`);
      });
  }
};
</script>

<template>
  <div class="event-card -shadow">
    <h4>{{ event.title }}</h4>
    <router-link :to="{ name: 'event-show', params: { id: event.id } }">
      <BaseIcon name="eye">Show event</BaseIcon>
    </router-link>
    <router-link :to="{ name: 'event-update', params: { id: event.id } }">
      <BaseIcon name="edit">Edit event</BaseIcon>
    </router-link>
    <BaseIcon name="trash">
      <span class="hand" v-on:click="deleteEvent(event)">Delete Event</span>
    </BaseIcon>
  </div>
</template>

<script>
import EventService from "@/services/EventService.js";
export default {
  props: {
    event: Object
  },
  methods: {
    deleteEvent(event) {
      alert(`You're deleting ${event.title} with id ${event.id}!!!`);
      EventService.deleteEvent(event.id)
        .then(response => {
          console.log(response.data + ": Event deleted.");
        })
        .catch(err => {
          console.log(`Error: ${err.response}`);
        });
      window.location.reload();
    }
  }
};
</script>

<style scoped>
h4 {
  color: green;
}
.hand {
  cursor: pointer;
}
</style>

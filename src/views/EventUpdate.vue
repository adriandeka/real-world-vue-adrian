<template>
  <div>
    <h1>Update Event</h1>
    <form class="review-form" @submit.prevent="onSubmit">
      <p>
        <label for="name">Id:</label>
        <input id="name" v-model="id" placeholder="name" />
      </p>
      <p>
        <label for="name">Title:</label>
        <input id="name" v-model="event.title" placeholder="name" />
      </p>
      <p>
        <label for="review">Description:</label>
        <textarea id="review" v-model="event.description"></textarea>
      </p>
      <span v-show="updated && !errors.length" class="success-info">
        Event updated with success!</span
      >
      <span v-show="errors.length" class="error-info"> Errors</span>
      <ul>
        <li v-for="error in errors" v-bind:key="error" class="error-info">
          {{ error }}
        </li>
      </ul>
      <p>
        <input type="submit" value="Submit" />
      </p>
    </form>
  </div>
</template>
<script>
import EventService from "@/services/EventService.js";
export default {
  props: ["id"],
  data() {
    return {
      event: {},
      errors: [],
      updated: false
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
  },
  methods: {
    onSubmit() {
      this.errors = [];
      if (this.event.id && this.event.title && this.event.description) {
        EventService.updateEvent(this.event)
          .then(response => {
            console.log(response.data + ": Event updated succesfully");
            this.updated = true;
          })
          .catch(err => {
            console.log(`Error: ${err.response}`);
          });
      } else {
        if (!this.event.id) this.errors.push("Id required.");
        if (!this.event.title) this.errors.push("Title required.");
        if (!this.event.description) this.errors.push("Description required.");
      }
    }
  }
};
</script>
<style scoped>
.error-info {
  color: red;
}
.success-info {
  color: green;
}
</style>

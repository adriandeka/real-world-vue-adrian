<template>
  <div>
    <h1>Create Event</h1>
    <form class="review-form" @submit.prevent="onSubmit">
      <p>
        <label for="name">Id:</label>
        <input id="name" v-model="id" placeholder="name" />
      </p>
      <p>
        <label for="name">Title:</label>
        <input id="name" v-model="title" placeholder="name" />
      </p>
      <p>
        <label for="review">Description:</label>
        <textarea id="review" v-model="description"></textarea>
      </p>
      <span v-show="errors.length" class="error-info"> Errors</span>
      <span v-show="created && !errors.length" class="success-info">
        Event created with success!</span
      >
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
  data() {
    return {
      id: null,
      title: null,
      description: null,
      errors: [],
      created: false
    };
  },
  methods: {
    onSubmit() {
      this.errors = [];
      if (this.id && this.title && this.description) {
        let newEvent = {
          id: this.id,
          title: this.title,
          description: this.description
        };
        EventService.createEvent(newEvent)
          .then(response => {
            console.log(response.data + ": Event created succesfully");
            this.created = true;
          })
          .catch(err => {
            console.log(`Error: ${err.response}`);
          });
        this.id = null;
        this.title = null;
        this.description = null;
      } else {
        if (!this.id) this.errors.push("Id required.");
        if (!this.title) this.errors.push("Title required.");
        if (!this.description) this.errors.push("Description required.");
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

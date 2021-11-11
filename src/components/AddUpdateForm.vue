<template>
  <div class="form_wrapper">
    <form @keyup.enter.prevent="validate()">
      <div v-if="errors.length > 0">
        <ul>
          <li v-for="error in errors" :key="error">{{ error }}</li>
        </ul>
      </div>
      <span class="close" @click="$emit('close-form')"> &#10060; </span>
      <div>
        <label for="name">Name:</label>
        <input type="text" id="name" v-model="eventObj.name" />
      </div>
      <div>
        <label for="details">Details:</label>
        <input type="text" id="details" v-model="eventObj.details" />
      </div>
      <div>
        <label for="date">Date:</label>
        <input type="date" id="date" v-model="eventObj.date" />
      </div>
      <div>
        <label for="background">background:</label>
        <select name="" id="background" v-model="eventObj.background">
          <option value="#F34949">Red</option>
          <option value="#F07AEC">Pink</option>
          <option value="#997AF0">Purple</option>
          <option value="#7AD3F0">Blue</option>
          <option value="#68EE94">Green</option>
          <option value="#F9F970">Yellow</option>
          <option value="#EB9A0F">Orange</option>
        </select>
      </div>
      <button v-if="currentEvent.id" @click.prevent="validate('update')">
        Update
      </button>
      <button v-else @click.prevent="validate('add')">Add</button>
    </form>
  </div>
</template>

<script>
export default {
  // props: ["currentEvent"],
  props: {
    currentEvent: {
      type: Object,
    },
  },
  emits: ["close-form", "push-event", "update-event"],
  data() {
    return {
      eventObj: {},
      errors: [],
    };
  },
  mounted() {
    this.eventObj = this.currentEvent;
  },
  methods: {
    addEvent() {
      this.$emit("push-event", this.eventObj);
      this.$emit("close-form");
    },
    updateEvent() {
      this.$emit("update-event", this.eventObj);
      this.$emit("close-form");
    },
    validate(type) {
      this.errors = [];
      if (!this.eventObj.name) {
        this.errors.push("Name is required");
      }
      if (!this.eventObj.details) {
        this.errors.push("Event details are required");
      }
      if (!this.eventObj.date) {
        this.errors.push("Event date is required");
      }
      if (!this.eventObj.background) {
        this.errors.push("Event background is required");
      }
      // form has errors, don't add event
      if (this.errors.length > 0) {
        return;
      }
      // no errors, add event to array
      if (type === "add") {
        this.addEvent();
      } else {
        // update the event array.
        this.updateEvent();
      }
    },
  },
};
</script>

<style scoped>
.close {
  align-self: flex-end;
  cursor: pointer;
}

.form_wrapper {
  background: rgba(0, 0, 0, 0.7);
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  display: flex;
  justify-content: center;
  align-items: center;
}

form {
  display: flex;
  flex-direction: column;
  background: darkgrey;
  min-width: 60vh;
  min-height: 40vh;
  padding: 2rem;
  border-radius: 0.3rem;
}

form > div {
  display: flex;
  flex-direction: column;
  margin: 1rem 0;
  font-size: 1.6rem;
}

form input,
select {
  margin: 0.6rem 0;
  padding: 0.6rem 1rem;
  border: 1px solid lightgrey;
  border-radius: 0.3rem;
}

form button {
  background-color: rgb(123, 194, 123);
  width: 100px;
  border: none;
  padding: 0.6rem;
  border-radius: 0.3rem;
  font-size: 1rem;
}

label {
  color: rgb(49, 49, 49);
}
</style>
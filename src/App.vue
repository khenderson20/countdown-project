<template>
  <teleport to="#modal">
    <AddUpdateForm v-if="showForm" />
  </teleport>
  <div id="logo">
    <img alt="Vue logo" src="./assets/logo.png" />
  </div>
  <div class="options">
    <button @click="showPastEvents = !showPastEvents">Show Past Events</button>
    <button class="addNew" @click="showForm = !showForm">&#43;</button>
  </div>
  <ul>
    <li v-for="el in orderEvents" :key="el.id">
      <Event
        :event="el"
        :daysLeft="daysLeft(el)"
        :showPastEvents="showPastEvents"
      ></Event>
    </li>
  </ul>
</template>

<script>
const eventData = [
  {
    id: 1,
    name: "Graduation",
    details: "wooohoo!!!",
    date: "2017-09-25", // past date
    background: "#F34949",
  },
  {
    id: 2,
    name: "Holidays",
    details: "wooohoo!!!",
    date: "2021-11-04", // today's date
    background: "#f9f970",
  },
  {
    id: 3,
    name: "HolidayYYYY",
    details: "Get me outta here!",
    date: "2021-11-05", // tomorrows date
    background: "#7AD3F0",
  },
  {
    id: 4,
    name: "Birthday",
    details: "My birthday party",
    date: "2024-01-22", // future date
    background: "#F07AEC",
  },
  {
    id: 5,
    name: "Christmas",
    details: "ho ho ho",
    date: "2020-03-5",
    background: "#EB9A0F",
  },
  {
    id: 6,
    name: "Conference Talk",
    details: "dont flop",
    date: "2021-02-28",
    background: "#68ee94",
  },
];
import Event from "./components/Event.vue";
import AddUpdateForm from "./components/AddUpdateForm.vue";
export default {
  name: "App",
  components: {
    Event,
    AddUpdateForm,
  },

  data() {
    return {
      events: eventData,
      showPastEvents: true,
      showForm: false,
    };
  },
  methods: {
    daysLeft(e) {
      const Time = Date.parse(e.date) - Date.now();
      // converting the milliseconds to proper Days using the miliseconds / (miliseconds * seconds * minutes * hours) <-- in 1 day
      const Days = Math.ceil(Time / (1000 * 60 * 60 * 24));
      console.log(Days);

      return Days;
    },
  },
  computed: {
    orderEvents() {
      const eventsToOrder = this.events;
      return eventsToOrder.sort((a, b) => (a.date > b.date ? 1 : -1));
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  max-width: 600px;
  margin: 0 auto;
}

#logo {
  text-align: center;
}

ul {
  padding: 0;
}

li {
  list-style: none;
  cursor: pointer;
}
</style>

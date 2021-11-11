<template>
  <teleport to="#modal">
    <transition name="bounce">
      <AddUpdateForm
        @push-event="add($event)"
        @update-event="update($event)"
        @close-form="closeForm()"
        v-if="showForm"
        :currentEvent="currentEvent"
      />
    </transition>
  </teleport>

  <div id="logo">
    <img alt="Vue logo" src="./assets/logo.png" />
  </div>
  <div class="options">
    <button @click="showPastEvents = !showPastEvents">Show Past Events</button>
    <button class="addNew" @click="showForm = !showForm">&#43;</button>
  </div>
  <ul>
    <transition-group
      name="fade"
      :appear="true"
      @before-enter="beforeEnter"
      @enter="enter"
      @leave="leave"
      move-class="fade-move"
    >
      <li
        v-for="(el, index) in orderEvents"
        :key="el.id"
        :data-index="index"
        @click="setForm(el)"
        class="fade-item"
      >
        <Event
          :event="el"
          :daysLeft="daysLeft(el)"
          :showPastEvents="showPastEvents"
          @remove-event="remove($event)"
        ></Event>
      </li>
    </transition-group>
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
import gsap from "gsap";
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
      currentEvent: {},
    };
  },
  methods: {
    findEventIndex(id) {
      return this.events.findIndex((element) => element.id === id);
    },
    remove(e) {
      // loop through and find the index in 'eventData' that matches the event id
      // remove the element based on index found
      this.events.splice(this.findEventIndex(e.id), 1);
    },
    closeForm() {
      this.showForm = false;
      this.currentEvent = {};
    },
    update(e) {
      // loop through and find the index in 'eventData' that matches the event id
      // set that found element with the NEW event data.
      this.events[this.findEventIndex(e.id)] = e;
    },
    setForm(e) {
      this.currentEvent = e || {};
      this.showForm = true;
    },
    add(e) {
      // add an id based on the array length
      e.id = this.events.length + 1;
      // push the new event onto array of events
      this.events.push(e);
      // close the form using custom event 'close-form' instead of using boolean
      // this.showForm = false;
    },
    daysLeft(e) {
      const Time = Date.parse(e.date) - Date.now();
      // converting the milliseconds to proper Days using the miliseconds / (miliseconds * seconds * minutes * hours) <-- in 1 day
      const Days = Math.ceil(Time / (1000 * 60 * 60 * 24));
      console.log(Days);

      return Days;
    },
    beforeEnter(el) {
      let tl = gsap.timeline(); // create timeline
      tl.fromTo(el, { opacity: 0 }, { opacity: 1 });
    },
    enter(el, done) {
      let tl = gsap.timeline(); // create timeline
      tl.from(el, {
        opacity: 1,
        ease: "power3",
        x: -1000,
        delay: el.dataset.index * 0.15,
      }) // start the sequence
        .set(el, {
          opacity: 1,
          ease: "power3",
          onComplete: done,
        });
    },
    leave(el, done) {
      let tl = gsap.timeline(); // create a timeline
      tl.set(el, { opacity: 1 }) // start the sequence
        .to(el, {
          opacity: 0,
          x: 3000,
          ease: "power3",
          duration: 0.8,
          onComplete: done,
        });
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

/* CSS Transitions for modal component render. */
.bounce-enter-active {
  animation: bounce-in 0.5s;
}

.bounce-leave-active {
  animation: bounce-in 0.5s reverse;
}

@keyframes bounce-in {
  0% {
    transform: scale(0);
    opacity: 0.3;
    z-index: 3;
  }
  100% {
    transform: scale(1);
    opacity: 1;
  }
}

/* CSS Transition for event array component renders. */
.fade-item {
  transition: all 0.8s ease;
}
.fade-leave-active {
  position: absolute;
  width: 40%;
}

.fade-move {
  transition: all 0.8s ease;
}
</style>

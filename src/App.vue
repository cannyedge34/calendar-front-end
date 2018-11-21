<template>
  <div id="app">
    <div class="main">
      <div class="calendar-holder">
        <calendar :events="events" />
      </div>
      <div class="form-holder">
        <h3>Schedule an event</h3>
        <event-form @update="allEvents" />
      </div>
    </div>
  </div>
</template>
<script>
import Calendar from './components/Calendar.vue'
import EventForm from './components/EventForm.vue'
export default {
  name: 'app',
  components: {
    Calendar,
    EventForm
  },
  data () {
    return {
      events: []
    }
  },
  methods: {
    buildEventsList (data) {
      this.events = data
    },
    allEvents (data) {
      this.events.push(data)
    }
  },
  created () {
    this.$http.get('http://localhost:3000/events')
      .then(res => {
        this.buildEventsList(res.data)
      })
      .catch(() => {
        alert('Something went wrong!')
      })
  }
}
</script>
<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
  .main {
    display: flex;
    align-items: center;
  }
  .calendar-holder {
    width: 65%;
  }
  .form-holder {
    width: 35%;
  }
  .form-holder > h3 {
    color: orangered;
    text-transform: uppercase;
    font-size: 16px;
    text-align: left;
    margin-left: 30px;
    margin-bottom: 10px;
  }
</style>

<template>
  <div>
    <full-calendar :events="events" @eventClick="viewDetail" />
    <v-dialog/>
  </div>
</template>
<script>
import FullCalendar from 'vue-fullcalendar'

export default {
  name: 'Calendar',
  props: ['events'],
  components: {
    FullCalendar
  },
  methods: {
    viewDetail (event) {
      this.$modal.show('dialog', {
        title: event.title,
        text: event.description
      })
    }
  }
}
</script>

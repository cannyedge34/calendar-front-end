<template>
  <form @submit.prevent="handleSubmit">
    <div class="input-holder">
      <input type="text" placeholder="Event title" v-model="event.title"/>
    </div>
    <div class="input-holder">
      <date-picker :placeholder="'Start date'" v-model="event.start_date" />
    </div>
    <div class="input-holder">
      <date-picker :placeholder="'End date'" v-model="event.end_date"/>
    </div>
    <div class="input-holder">
      <textarea placeholder="Event description" rows="4" v-model="event.description" ></textarea>
    </div>
    <div class="input-holder">
      <button type="submit">Schedule</button>
    </div>
  </form>
</template>

<script>
import DatePicker from 'vuejs-datepicker'
import format from 'date-fns/format'
export default {
  name: 'EventForm',
  data () {
    return {
      event: {
        title: '',
        start_date: '',
        end_date: '',
        description: ''
      }
    }
  },
  methods: {
    handleSubmit () {
      const startDate = format(this.event.start_date, 'YYYY-MM-DD')
      const endDate = format(this.event.end_date, 'YYYY-MM-DD')
      const event = {
        ...this.event,
        start_date: startDate,
        end_date: endDate
      }
      this.$http.post('http://localhost:3000/events', event)
        .then(response => {
          this.$emit('update', response.data)
          this.resetValues()
        }).catch(err => {
          alert(err.response.data.message)
        })
    },
    resetValues () {
      this.event = {
        title: '',
        start_date: '',
        end_date: '',
        description: ''
      }
    }
  },
  components: {
    DatePicker
  }
}
</script>

<style>
  form {
    display: flex;
    flex-direction: column;
    margin-left: 30px;
  }
  .input-holder {
    margin: 10px 0;
    display: flex;
    justify-content: flex-start;
  }
  .vdp-datepicker {
    width: 100%;
  }
  .vdp-datepicker > div > input {
    width: 77%;
  }
  .input-holder > button {
    justify-self: center;
    padding: 12px 25px;
    border-radius: 0;
    text-transform: uppercase;
    font-weight: 600;
    background: orangered;
    color: white;
    border: none;
    font-size: 14px;
    letter-spacing: -0.1px;
    cursor: pointer;
  }
  input,
  textarea {
    padding: 12px 15px;
    border: 2px solid rgba(0, 0, 0, 0.2);
    border-radius: 0;
    width: 70%;
    opacity: 0.8;
    font-size: 15px;
    font-weight: normal;
  }
  input:focus,
  textarea:focus,
  button:focus {
    border: 2px solid orangered;
    outline: none;
    box-shadow: 0 2px 3px 1px rgba(0, 0, 0, 0.2);
  }
</style>

<template lang="html">
  <div class="booking">
    <h2>{{booking.name}}'s booking</h2>
    <p>E-mail:{{booking.email}}</p>
    <p v-if="getStatus === true">Guest Status: Checked In</p>
    <p v-if="getStatus === false">Guest Status: Checked Out</p>
    <button v-on:click="handleDelete">Delete Booking</button>
    <button v-on:click="handleUpdate">Update Guest Status</button>
  </div>

</template>

<script>
import HotelService from '../services/HotelService';
import { eventBus } from '../main';

export default {
  name: "booking",
  props: ['booking'],
  methods: {
    handleDelete(){
      HotelService.deleteBooking(this.booking._id)
      .then(() => eventBus.$emit('booking-deleted', this.booking._id));
    },
    handleUpdate(e){
      e.preventDefault()
      if (this.booking.checked === true) {
        HotelService.updateBooking(this.booking._id, {checked: false})
        .then(response => eventBus.$emit('booking-true', this.booking._id));
      } else {
        HotelService.updateBooking(this.booking._id, {checked: true})
        .then(response => eventBus.$emit('booking-false', this.booking._id));
      }
    }
  },
  computed: {
    getStatus: function(){
      return this.booking.checked
    }
  }

}
</script>

<style lang="css" scoped>
.booking {
  width: 30%;
  background: rgba(255,255,255, 0.7);
  margin-bottom: 20px;
  padding: 25px;
}
</style>

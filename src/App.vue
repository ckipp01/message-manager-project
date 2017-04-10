<template>
  <div id="app">
    <h1>Message Manager</h1>
    <el-row>
      <el-col :span="24">
        <h3>Hotel View</h3>
        <el-row>
          <el-col :span="8">
            <h4>Hotels</h4>
            <el-select v-model="company" placeholder="Select a Hotel">
              <el-option
                v-for="Company in Companies"
                :key="Company.id" 
                :label="Company.company"
                :value="Company">
              </el-option>
            </el-select>
          </el-col>
          <el-col :span="8">
            <h4>Guests</h4>
            <el-select v-model="guest" placeholder="Select a Guest">
              <el-option
                v-for="Guest in Guests"
                :key="Guest.id" 
                :label="Guest.lastName + ', ' + Guest.firstName"
                :value="Guest">
              </el-option>
            </el-select>
          </el-col>
          <el-col :span="8">
            <h4>Message Type</h4>
            <el-select v-model="message" placeholder="Select a Message">
              <el-option
                v-for="Message in messageTypes"
                :key="Message.id" 
                :label="Message.label"
                :value="Message.value">
              </el-option>
            </el-select>
          </el-col>
        </el-row>
        <el-row class="buttonRow">
            <el-button :plain="true" @click="sendMessage">Verify information below and click to send message</el-button>
        </el-row>
          <p v-if="company != null"> {{ company.company }} {{company.timezone}}</p>
          <p v-if="guest != null"> {{ guest.firstName}} {{ guest.lastName }}</p>
          <p v-if="message != null"> {{ message }} </p>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import Companies from './assets/Companies.json'
import Guests from './assets/Guests.json'
const moment = require('moment');
const tz = require('moment-timezone');
moment.tz.add('US/Western|PST PDT|80 70|0101|1Lzm0 1zb0 Op0');

export default {
  name: 'app',
  data () {
    return {
      Companies: Companies,
      Guests: Guests,
      messageTypes: [{
        id: 1,
        value: "Check In",
        label: "Check In"
      }, 
      {
        id: 2,
        value: "Check Out",
        label: "Check Out"
      },],
      company: '',
      guest: '',
      message: '',
      checkInGreeting: '',
      checkOutGreeting: ''
    }
  },
  methods: {
    sendMessage() {
      if (this.guest !== '') {
          this.setGreetings();
      }
      if (this.message === 'Check In' && this.company !== null && this.guest !== null) {
          this.$message({
            message: this.checkInGreeting + this.guest.firstName + ', and welcome to ' + this.company.company + '! Room ' + this.guest.reservation.roomNumber + ' is now ready you. Enjoy your stay, and let us know if you need anything.',
            type: 'success'
          });
      } else if (this.message === 'Check Out' && this.company !== null && this.guest !== null) {
          this.$message({
            message: this.checkOutGreeting + this.guest.firstName + ', and thank you for staying at ' + this.company.company + '! We hope your stay in Room ' + this.guest.reservation.roomNumber + ' was pleasant, and we hope to see you again.',
            type: 'success'
          });
      } else {
          this.$message({
            message: 'Make sure all fields are filled out',
            type: 'warning'
          })
      }
    },
    setGreetings() {
      if (moment.tz(this.guest.reservation.startTimestamp,this.company.timezone).format("HH") >= 0 && moment.tz(this.guest.reservation.startTimestamp,this.company.timezone).format("HH") < 12) {
          this.checkInGreeting = 'Good morning ';
      } else if (moment.tz(this.guest.reservation.startTimestamp,this.company.timezone).format("HH") >= 12 && moment.tz(this.guest.reservation.startTimestamp,this.company.timezone).format("HH") < 17) {
          this.checkInGreeting = 'Good afternoon ';
      } else if (moment.tz(this.guest.reservation.startTimestamp,this.company.timezone).format("HH") >= 17 && moment.tz(this.guest.reservation.startTimestamp,this.company.timezone).format("HH") < 24) {
          this.checkInGreeting = 'Good evening ';
      } else {
        this.checkInGreeting = 'Hello ';
      }
      if (moment.tz(this.guest.reservation.endTimestamp,this.company.timezone).format("HH") >= 0 && moment.tz(this.guest.reservation.endTimestamp,this.company.timezone).format("HH") < 12) {
          this.checkOutGreeting = 'Good morning ';
      } else if (moment.tz(this.guest.reservation.endTimestamp,this.company.timezone).format("HH") >= 12 && moment.tz(this.guest.reservation.endTimestamp,this.company.timezone).format("HH") < 17) {
          this.checkOutGreeting = 'Good afternoon ';
      } else if (moment.tz(this.guest.reservation.endTimestamp,this.company.timezone).format("HH") >= 17 && moment.tz(this.guest.reservation.endTimestamp,this.company.timezone).format("HH") < 24) {
          this.checkOutGreeting = 'Good evening ';
      } else {
        this.checkOutGreeting = 'Hello';
      }
    }
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
.buttonRow {
  padding-top:5%;
}
</style>

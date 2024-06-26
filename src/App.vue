<script>

export default {
  name: 'App',
  data() {
    return {
      framelist: [],
      myindex: 0,
      inputjonumber: ""
    }
  },
  computed: {
    findResults() {
      let index
      console.log(this.framelist.values)
      for (var i = 0; i < this.framelist.values.length; i++) {
        for (var k = 0; k < this.framelist.values[0].length; k++) {
          if (this.framelist.values[i][k] == this.inputjonumber) {
            index = i
            this.myindex = index
          }
        }
      }
    },
  },
  methods: {
    async getData() {
      const res = await fetch("https://sheets.googleapis.com/v4/spreadsheets/186-7MMQBauKDWO4E9Vk-4hbVhTVPYtMHRSjRK9ZITpQ/values/In-Studio%20Shoots/?alt=json&key=AIzaSyCkPCG3NLQ4bMpRQsDPir9mKFgKvybmQvE")
      const finalRes = await res.json()
      this.framelist = finalRes
    }
  },
  components: {
  },
  mounted() {

    this.getData()

  }
}

</script>

<template>
  <header></header>

  <main>
    <div class="wrapper">
      <div style="text-align: center;"> <B>FRAME AVAILABILITY</B> </div> <hr>
      <div> <input v-model="inputjonumber" placeholder="Input your J.O. number"> <button id="findButton" @click="findResults">Check</button> </div>

      <div v-if="myindex === 0"> <b>J.O. Number:</b> {{ inputjonumber }} </div>
      <div v-else>
        <b>J.O. Number: </b> {{ inputjonumber }} <br>
        <b>Name: </b> {{ framelist.values[this.myindex][0] }}<br>
        <b>Ready for Pickup: </b>
        
        <span v-if="framelist.values[this.myindex][5]=='yes'" style="color:green">
          ✔
        </span>
        <span v-else style="color:red">
          ✘
        </span>


      </div>
    </div>
    <div v-if="myindex === 0"> </div>
    <div v-else>
      <div v-if="framelist.values[this.myindex][5]=='yes'">
        <div class="wrapper">

          Your photo package is ready for pick-up! Please text Mars at least one day before you plan to pick up your package or send over a courier. This would be highly appreciated! <br><br>
          Contact Details:<br><br>
          Name: Dalyn<br>
          Contact Number: +63 917 577 3406 <br>
          Hours: 9AM-4PM <br>
          Address: Stall LA-16 Hobbies of Asia Mall #8 Diosdado Macapagal Blvd. Pasay City   <br>
          <i style="color: #242320; font-size:smaller">*We have a pin on Grab. Just type “TinoLey Digital Photography, Inc.”</i><br><br>
          Note: Please text Dalyn 1-day before you pick up your photo package so she can prepare it! <br>

        </div>
      </div>
      <div v-else>
        <div class="wrapper">

          Your frame is not ready yet. Please check again later.

        </div>
      </div>
    </div>

  </main>
</template>

<style scoped>

.wrapper{
  border: 3px solid aliceblue;
  background-color: gainsboro;
  padding-left: 2%;
  padding-top: 2%;
  padding-bottom: 2%; 
  padding-right: 2%;
  width: 20%;
  font-family: Arial, Helvetica, sans-serif;
}

input{
  margin-bottom:5%;
}

</style>
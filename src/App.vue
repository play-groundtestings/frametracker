<script>

export default {
  name: 'App',
  data() {
    return {
      numberdata: 68,
      framelist: [],
      myindex: 0,
      text: ""
    }
  },
  computed: {
    findResults() {
      let index
      for (var i = 0; i < this.framelist.values.length; i++) {
        for (var k = 0; k < this.framelist.values[0].length; k++) {
          if (this.framelist.values[i][k] == this.text) {
            index = i
            this.myindex = index
          }
        }
      }
    },
  },
  methods: {
    async getData() {
      const res = await fetch("https://sheets.googleapis.com/v4/spreadsheets/186-7MMQBauKDWO4E9Vk-4hbVhTVPYtMHRSjRK9ZITpQ/values/woodrose/?alt=json&key=AIzaSyCkPCG3NLQ4bMpRQsDPir9mKFgKvybmQvE")
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
      <div> <input v-model="text" placeholder="Input your J.O. number"> <button id="findButton" @click="findResults">Check</button> </div>
      <div v-if="myindex === 0"> <b>J.O. Number:</b> {{ text }} </div>
      <div v-else>
        <b>J.O. Number: </b> {{ text }} <br>
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
  </main>
</template>

<style scoped>

.wrapper{
  border: 3px solid aliceblue;
  background-color: gainsboro;
  padding-left: 2%;
  padding-top: 2%;
  padding-bottom: 2%;
  width: 20%;
  font-family: Arial, Helvetica, sans-serif;
}

input{
  margin-bottom:5%;
}

</style>
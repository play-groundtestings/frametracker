<script>
import HelloWorld from './components/HelloWorld.vue'
import TheWelcome from './components/TheWelcome.vue'

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
    findResults(){
      let index
      for(var i = 0; i<this.framelist.values.length;i++){
        for(var k = 0; k<this.framelist.values[0].length;k++){
          if(this.framelist.values[i][k] == this.text){
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
  <header>

    <div class="wrapper">
      <HelloWorld msg="You did it!" />

    </div>
    <br>
    <div id="content"></div>
    <div> {{ numberdata + 1 }}</div>
    <hr>
    <div> {{ framelist.values}}</div>
    <div> <input v-model="text"> <button id="findButton" @click="findResults">Check</button> </div>
    <div v-if="myindex!==0"> <b>Name:  </b> {{ framelist.values[this.myindex][0] }} <b>J.O. Number</b> {{ text }} <b>Ready for Pickup: </b>  {{framelist.values[this.myindex][5]}}</div>
    <div v-else><b>J.O Number</b> {{ text }}</div>
  </header>

  <main>
    <TheWelcome />
  </main>
</template>

<!--
  
  https://sheets.googleapis.com/v4/spreadsheets/186-7MMQBauKDWO4E9Vk-4hbVhTVPYtMHRSjRK9ZITpQ/values/woodrose/?alt=json&key=AIzaSyCkPCG3NLQ4bMpRQsDPir9mKFgKvybmQvE
  <b>Name:  </b> {{ framelist.values[this.myindex][0] }} <b>J.O. Number</b> {{ framelist.values[this.myindex][2] }} <b>Ready for Pickup: </b>  {{framelist.values[this.myindex][5]}}
-->
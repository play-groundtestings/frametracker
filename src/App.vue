<script>
export default {
  name: 'App',

  data() {
    return {
      framelist: [],        // full sheet data
      myindex: -1,          // row index, -1 = not found
      inputjonumber: "",    // user input
      hasSearched: false    // whether user pressed Check
    }
  },

  methods: {
    async getData() {
      try {
        const res = await fetch(
          "https://sheets.googleapis.com/v4/spreadsheets/186-7MMQBauKDWO4E9Vk-4hbVhTVPYtMHRSjRK9ZITpQ/values/In-Studio%20Shoots/?alt=json&key=AIzaSyCkPCG3NLQ4bMpRQsDPir9mKFgKvybmQvE"
        );
        const finalRes = await res.json();
        this.framelist = finalRes;
      } catch (e) {
        console.error("Failed to load sheet:", e);
        this.framelist = { values: [] };
      }
    },

    findResults() {
      this.hasSearched = true;  // mark that user clicked Check
      this.myindex = -1;        // reset index

      if (!this.framelist.values) return;

      // JO Number is column index 2
      for (let i = 0; i < this.framelist.values.length; i++) {
        const row = this.framelist.values[i];
        if (row[2] && row[2].toString() === this.inputjonumber.toString()) {
          this.myindex = i;
          return;
        }
      }

      // intentionally do not show any message if no match
    }
  },

  mounted() {
    this.getData();
  }
}
</script>

<template>
  <header></header>

  <main>
    <div class="wrapper">
      <div style="text-align: center;"><b>FRAME AVAILABILITY</b></div>
      <hr>

      <div>
        <input v-model="inputjonumber" placeholder="Input your J.O. number">
        <button id="findButton" @click="findResults">Check</button>
      </div>

      <!-- SHOW RESULTS ONLY IF FOUND -->
      <div v-if="hasSearched && myindex >= 0">
        <b>J.O. Number:</b> {{ inputjonumber }} <br>
        <b>Name:</b> {{ framelist.values[myindex][0] }}<br>

        <b>Ready for Pickup:</b>
        <span v-if="framelist.values[myindex][5] === 'yes'" style="color:green">✔</span>
        <span v-else style="color:red">✘</span>
      </div>
    </div>

    <!-- READY FOR PICKUP MESSAGE (only when found) -->
    <div v-if="hasSearched && myindex >= 0">
      <div v-if="framelist.values[myindex][5] === 'yes'">
        <div class="wrapper">
          Your photo package is ready for pick-up! Please text Dalyn at least one day before you plan to pick up your package or send over a courier (Grab/Lalamove).<br><br>

          <b>Pick-Up Details:</b><br><br>
          Name: Dalyn <br>
          Contact Number: +63 917 577 3406 <br>
          Hours: 9AM-4PM <br>
          Address: 2nd Floor, 105-E Mariano Ponce St., Caloocan City <br><br>

          Note: Please text Dalyn 1-day before you pick up your photo package so she can prepare it!
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
.wrapper {
  border: 3px solid aliceblue;
  background-color: gainsboro;
  padding: 2%;
  width: 20%;
  font-family: Arial, Helvetica, sans-serif;
}
input {
  margin-bottom: 5%;
}
</style>

<template>
  <div id="box">
    <span style="font-size: 50px; color: #B1B1B1">Currency Converter</span>
    <hr />
    <span>Enter Amount: &nbsp;</span>
    <input type="number" v-model.number="amount" placeholder="Enter Amount" />
    <br />
    <br />
    <span>Convert From: &nbsp;</span>

    <select v-model="convertfrom">
      <option
        v-for="(a, index) in currencyfrom"
        v-bind:value="a.name"
        v-bind:key="index"
        v-bind:name="a.desc"
      >{{a.desc}}</option>
    </select>

    <span>&nbsp;&nbsp;Convert To: &nbsp;</span>
    <select v-model="convertto">
      <option
        v-for="(a, index) in currencyfrom"
        v-bind:value="a.name"
        v-bind:key="index"
        v-bind:name="a.desc"
      >{{a.desc}}</option>
    </select>
    <br />
    <br />
    <span>{{amount}} {{convertfrom}} equals {{finalamount}} {{convertto}}</span>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "App",
  data() {
    return {
      name: "dropdown",
      currencyfrom: [
        { name: "INR", desc: "INR" },
        { name: "MYR", desc: "MYR" }
      ],
      convertfrom: "INR",
      convertto: "INR",
      amount: ""
    };
  },
  mounted() {
    axios
      .get(
        "https://g9jgss86gg.execute-api.us-east-1.amazonaws.com/test/currency/details?currency_code=MYR"
      )
      .then(response => {
        this.convertfrom = response.data.payload.code;
      });
  },
  computed: {
    finalamount: function() {
      var to = this.convertto;
      var from = this.convertfrom;
      var final;
      switch (from) {
        case "MYR":
          {
            if (to == "INR") {
              final = (this.amount * 17).toFixed(2);
            }
          }
          break;
        case "INR":
          {
            if (to == "MYR") {
              final = this.amount * (0.05).toFixed(2);
            }
          }
          break;
      }
      return final;
    }
  }
};
</script>

<style>
@import url("https://fonts.googleapis.com/css?family=Source+Sans+Pro&display=swap");
body {
  text-align: center;
  margin-left: 240px;
  margin-right: 240px;
  margin-top: 100px;
  height: 450px;
}
#box {
  background: #ececec;
  padding: 30px;
  border-top: 12px solid skyblue;
  box-shadow: 10px 10px rgba(0, 0, 0, 0.3);
}

select {
  width: 100px;
  font-size: 25px;
}
span,
option,
input {
  font-size: 25px;
}
option,
span,
h1,
h2 {
  font-family: "Source Sans Pro", sans-serif;
}

input {
  border-radius: 6px;
}
@media (max-width: 800px) {
  body {
    text-align: center;
    margin-left: 50px;
    margin-right: 50px;
    margin-top: 50px;
    height: 200px;
  }
}
</style>

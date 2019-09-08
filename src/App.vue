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
        v-for="(currency, index) in currencies"
        v-bind:value="currency.name"
        v-bind:key="index"
        v-bind:name="currency.desc"
      >{{currency.desc}}</option>
    </select>
    <span></span>
    <span>&nbsp; &nbsp;Convert To: &nbsp;</span>
    <select v-model="convertto">
      <option
        v-for="(currency, index) in currencies"
        v-bind:value="currency.name"
        v-bind:key="index"
        v-bind:name="currency.desc"
      >{{currency.desc}}</option>
    </select>
    <br />

    <div class="button_cont" align="center">
      <a
        v-on:click.prevent="convert"
        id="convert-button"
        class="button_style"
        href="add-website-here"
        target="_blank"
        rel="nofollow noopener"
      >
        <b>Convert</b>
      </a>
    </div>

    <br />
    <br />
    <span>{{amount}} {{convertfrom}} equals {{ convertedCurrencyRate }} {{convertto}}</span>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "App",
  data() {
    return {
      name: "dropdown",
      currencies: [
        { name: "EUR", desc: "EUR" },
        { name: "MYR", desc: "MYR" },
        { name: "USD", desc: "USD" }
      ],
      convertfrom: "",
      convertto: "",
      amount: "",
      fromSpotRate: 0,
      toSpotRate: 0,
      final: ""
    };
  },

  computed: {
    convertedCurrencyRate() {
      if (this.fromSpotRate === 0 || this.toSpotRate === 0) {
        return "(No conversion made)";
      }

      // the currency calucations here based on the spot rates.
      // you can directly return the output
      // it will be rendered in {{ convertedCurrencyRate }} on the template

      if (
        this.fromSpotRate < this.toSpotRate ||
        this.fromSpotRate > this.toSpotRate
      ) {
        //  the necessary calculations are here.
        // and return the value

        return ((this.amount / this.fromSpotRate) * this.toSpotRate).toFixed(2);
      }

      //  from currency and to currency are same then result
      if (this.fromSpotRate === this.toSpotRate) {
        return this.amount;
      }
    }
  },

  methods: {
    convert() {
      axios
        .get(
          `https://g9jgss86gg.execute-api.us-east-1.amazonaws.com/test/currency/details?currency_code=${this.convertfrom}`
        )
        .then(response => {
          this.fromSpotRate = response.data.payload.spot_rate;
        });

      axios
        .get(
          `https://g9jgss86gg.execute-api.us-east-1.amazonaws.com/test/currency/details?currency_code=${this.convertto}`
        )
        .then(response => {
          this.toSpotRate = response.data.payload.spot_rate;
        });
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

#convert-button {
  padding: 15px;
  background-color: #008ecc;
  color: white;
  border: none;
  margin: 3rem;
}
.button_style {
  color: #fff !important;
  text-transform: uppercase;
  text-decoration: none;
  padding: 20px;
  border-radius: 5px;
  display: inline-block;
  border: none;
  transition: all 0.4s ease 0s;
}
.button_style:hover {
  letter-spacing: 1px;
  -webkit-box-shadow: 0px 5px 40px -10px rgba(0, 0, 0, 0.57);
  -moz-box-shadow: 0px 5px 40px -10px rgba(0, 0, 0, 0.57);
  box-shadow: 5px 40px -10px rgba(0, 0, 0, 0.57);
  transition: all 0.4s ease 0s;
}
</style>





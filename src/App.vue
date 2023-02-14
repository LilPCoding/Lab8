<template>
  <div class="header-bg header">
    <h1 class="text-h1">Welcome</h1>
    <button type="button" class="btn btn-danger" @click="getRates">
      Click this button first for start !!
    </button>

    <!-- <ul v-if="rates">
      <h5>เรท 1 USD ต่อคู่เงินอื่นๆ</h5>
      <li v-for="(rate, currency) in rates" :key="currency">
        {{ currency }}: {{ rate }}
      </li>
    </ul> -->
  </div>
  <!-- <hr /> -->
  <div class="body">
    <h2>แปลงสกุลเงิน</h2>
    <div class="card body shadow">
      <form @submit.prevent="convertCurrency">
        <div>
          <label for="amount">Amount :</label>
          <input
            class="form-control"
            type="number"
            id="amount"
            v-model.number="amount"
            :disabled="rates < 1"
          />
        </div>
        <div>
          <label for="from">From :</label>
          <select
            id="from"
            class="form-select"
            v-model="fromCurrency"
            :disabled="rates < 1"
          >
            <option v-for="currency in currencies" :key="currency">
              {{ currency }}
            </option>
          </select>
        </div>
        <div>
          <label for="to">To :</label>
          <select
            id="to"
            class="form-select"
            v-model="toCurrency"
            :disabled="rates < 1"
          >
            <option v-for="currency in currencies" :key="currency">
              {{ currency }}
            </option>
          </select>
        </div>
        <button
          type="submit"
          class="btn btn-primary"
          :disabled="rates < 1"
          style="margin: 10px 0px 10px 0px"
          @click="alertDisplay"
        >
          Convert
        </button>
      </form>
      <div class="alert alert-success" role="alert" v-if="convertedAmount">
        {{ amount }} {{ fromCurrency }} เท่ากับ {{ convertedAmount }}
        {{ toCurrency }}
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      rates: null,
      amount: 1,
      fromCurrency: "USD",
      toCurrency: "THB",
      convertedAmount: null,
    };
  },
  computed: {
    currencies() {
      return Object.keys(this.rates || {});
    },
  },
  methods: {
    async getRates() {
      try {
        const response = await axios.get(
          "https://fixer-fixer-currency-v1.p.rapidapi.com/latest?base=USD&symbols=THB%2CUSD%2CGBP%2CJPY%2CEUR",
          {
            headers: {
              "X-RapidAPI-Key":
                "c098f7becamsh3231080020a09dfp1db150jsna76ff3dafd74",
              "X-RapidAPI-Host": "fixer-fixer-currency-v1.p.rapidapi.com",
            },
          }
        );
        this.rates = response.data.rates;
      } catch (error) {
        console.error(error);
      }
    },
    convertCurrency() {
      this.convertedAmount =
        (this.amount / this.rates[this.fromCurrency]) *
        this.rates[this.toCurrency];
    },
  },
};
</script>

<style>
.body {
  display: grid;
  padding: 1rem 2rem 0rem 2rem;
  justify-content: center;
  align-items: center;
  height: 100%;
}

.header-bg {
  background-image: url("https://images.unsplash.com/photo-1614028674026-a65e31bfd27c?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1470&q=80");
  background-position: center;
  background-size: cover;
  background-repeat: no-repeat;
}

.header {
  display: grid;
  padding: 2rem;
  justify-content: center;
  align-items: center;
  height: 100%;
}

.text-h1 {
  color: white;
  line-height: 1.5;
  letter-spacing: 3px;
  word-spacing: 0px;
  text-align: center;
  text-decoration: none;
  text-transform: none;
  font-weight: bold;
}
</style>

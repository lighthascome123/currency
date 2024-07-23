<template>
  <div class="currency-converter p-8 w-full max-w-md mx-auto bg-white rounded-xl shadow-md space-y-4">
    <h1 class="text-2xl font-bold mb-4">Currency Converter</h1>
    <form @submit.prevent="convertCurrency" class="space-y-4">
      <div class="flex flex-col">
        <label for="from" class="mb-2">From:</label>
        <select v-model="from" id="from" class="border border-gray-300 p-2 rounded">
          <option v-for="currency in currencies" :key="currency" :value="currency">{{ currency }}</option>
        </select>
      </div>
      <div class="flex flex-col">
        <label for="to" class="mb-2">To:</label>
        <select v-model="to" id="to" class="border border-gray-300 p-2 rounded">
          <option v-for="currency in currencies" :key="currency" :value="currency">{{ currency }}</option>
        </select>
      </div>
      <div class="flex flex-col">
        <label for="amount" class="mb-2">Amount:</label>
        <input type="number" v-model="amount" id="amount" class="border border-gray-300 p-2 rounded" required>
      </div>
      <button type="submit" class="w-full bg-blue-500 text-white p-2 rounded">Convert</button>
    </form>
    <div v-if="convertedAmount !== null" class="mt-4 text-lg">
      Converting {{ amount }} {{ from }} to {{ to }}<br>
      Converted Amount: {{ convertedAmount.toFixed(2) }}
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      from: 'USD',
      to: 'EUR',
      amount: 1,
      convertedAmount: null,
      currencies: [
        'USD', 'EUR', 'GBP', 'JPY', 'AUD', 'CAD', 'CHF', 'CNY', 'SEK', 'NZD',
        'INR', 'BRL', 'RUB', 'ZAR', 'SGD', 'HKD', 'NOK', 'KRW', 'TRY', 'MXN',
        'IDR', 'MYR', 'THB', 'VND', 'PKR', 'EGP', 'PLN', 'HUF', 'CZK', 'DKK',
        'ILS', 'SAR', 'QAR', 'AED', 'KWD', 'BHD', 'OMR', 'JOD', 'LBP', 'MAD',
        'DZD', 'TND', 'ARS', 'CLP', 'COP', 'PEN', 'UYU', 'VEB', 'GHS', 'KES',
        'NGN', 'TZS', 'UGX', 'XAF', 'XOF', 'ZMK', 'MZN', 'ETB', 'AOA'
      ],
    };
  },
  methods: {
    async convertCurrency() {
      try {
        const response = await axios.get('http://localhost:3000/api/convert', {
          params: {
            from: this.from,
            to: this.to,
            amount: this.amount,
          },
        });
        this.convertedAmount = response.data.convertedAmount;
      } catch (error) {
        console.error('Error converting currency:', error);
      }
    },
  },
};
</script>

<style scoped>
.currency-converter {
  font-family: Avenir, Helvetica, Arial, sans-serif;
}
</style>

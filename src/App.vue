<template>
  <div id="app">
    <header class="header">
      <img
        class="header__logo"
        src="./assets/logo.svg"
        alt="logo that displays white plane on blue circle"
      />
    </header>
    <main class="content">
      <FilterCard 
        v-bind:filter="filterState"
        v-on:currency-changed="handleCurrencyChange($event)"
        v-on:only="handleOnlyStop($event)"
      />

      <TicketsList
        v-bind:rawTickets="filterTickets"
        v-bind:currencyRates="rates"
        v-bind:cur="filterState.currency"
      />
    </main>
  </div>
</template>

<script>
import FilterCard from './components/FilterCard.vue';
import TicketsList from './components/TicketsList.vue';
import { tickets as rawTickets } from './api/tickets'
import axios from 'axios';

export default {
  name: 'App',

  components: {
    FilterCard,
    TicketsList,
  },

  data() {
    return {
      rawTickets,
      filteredTickets: rawTickets,

      rates: {
        RUB: { sym: '₽', val: 1 },
        USD: { sym: '$', val: 70.73 },
        EUR: { sym: '€', val: 79.89 },
      },

      filterState: {
        currency: 'RUB',

        stops: {
          0: true,
          1: true,
          2: true,
          3: true,
          all: true,
        },
      },
    };
  },

  methods: {
    handleCurrencyChange(currency) {
      this.filterState.currency = currency;
    },

    handleOnlyStop(val) {
      const stops = this.filterState.stops;

      for (const key in stops) {
        stops[key] = false;
      }

      stops[val] = true;
      this.filterState.stops = stops;
    },
  },

  computed: {
    filterTickets: function() {
      const tickets = this.rawTickets;
      const stops = this.filterState.stops;
      const filteredTickets = tickets.filter(ticket => {

        for (const key in stops) {
          if (ticket.stops === +key && stops[key]) {
            return true;
          }
        }

        return false;
      });

      return filteredTickets;
    },
  },

  created() {
    axios.get('http://data.fixer.io/api/latest?access_key=75717cf964abe30c69db800e618eda19&symbols=USD,RUB&format=1')
      .then(response => {
        const rates = response.data.rates;
        this.rates.EUR.val = rates.RUB;
        this.rates.USD.val = rates.RUB / rates.USD;
        console.log('get from server:');
        console.log('USD: ', this.rates.USD.val, 'EUR: ', this.rates.EUR.val);
      })
      .catch(error => console.log(error));
}     

}
</script>

<style lang="scss">
  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  html {
    font-family: "Open Sans", Arial, Helvetica, sans-serif;
    font-size: 16px;
    line-height: 1.4;

    background-color: #f2f2f2;
  }

  .header {
    height: fit-content;
    padding-top: 50px;

    &__logo {
      
      display: block;
      margin: 0 auto;
      box-shadow: 0px 8px 11px rgba(55, 104, 142, 0.25);
      border-radius: 50%;
    }
  }

  .content {
    width: 820px;
    margin: 50px auto 0;
    display: flex;
    justify-content: space-between;
  }

  @media (max-width: 840px) {
    .content {
      width: fit-content;
      flex-direction: column;
      align-items: center;
    }
  }
</style>

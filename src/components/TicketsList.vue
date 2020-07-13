<template>
  <transition-group
    name="staggered-fade"
    class="tickets"
    tag="section"
    v-bind:css="false"
    v-on:before-enter="beforeEnter"
    v-on:enter="enter"
    v-on:leave="leave"
  >
    <TicketCard
      v-for="(ticket, ind) in filteredTickets"
      v-bind:data-index="ind"
      v-bind:key="ticket.id"
      v-bind:ticket="ticket"
      v-bind:currency="currency"
    />
  </transition-group>
</template>

<script>
import TicketCard from './TicketCard.vue'
import {v4 as uuid} from 'uuid';
import Velocity from 'velocity-animate';

  const formatDateItl = (rawDate, locale) => {
    const inputDate = new Date(rawDate);
    const month = inputDate
      .toLocaleString(locale, { month: 'short' })
      .replace('.', '');
    const weekday = inputDate
      .toLocaleString(locale, { weekday: 'short' });
    const date = inputDate.getDate();
    const year = inputDate.getFullYear();
    
    return `${date} ${month} ${year}, ${weekday}`;
  };

  const formatStopsRu = stop => {
    const pronounce = {
      0: 'Без пересадок',
      1: '1 пересадка',
      2: '2 пересадки',
      3: '3 пересадки',
    };

    return pronounce[stop];
  };

  const formatPrice = (priceValue, rates) => {
    const exchanges = Object.entries(rates);
    const formattedPrices = {};

    exchanges.forEach(exchange => {
      const [ currency, { sym, val } ] = exchange;
      const exPrice = Math.round(priceValue / val);
      const stringPrice = ('' + exPrice);
      const stringPriceLength = stringPrice.length;
      let formattedPrice = '';

      for (let i = 1; i <= stringPriceLength; i += 1) {
        formattedPrice = (i % 3 - 1)
          ? stringPrice[stringPriceLength - i] + formattedPrice
          : stringPrice[stringPriceLength - i] + ' ' + formattedPrice;
      }

      formattedPrices[currency] = `${formattedPrice}${sym}`
    });

    return formattedPrices;
  };

  const formatTicketFields = (ticket, locale, rates) => {
    const {
      arrival_date,
      departure_date,
      stops,
      price,
    } = ticket;
    const formattedArrivalDate = formatDateItl(arrival_date, locale);
    const formattedDepartureDate = formatDateItl(departure_date, locale);
    const formattedStops = formatStopsRu(stops);
    const formattedPrice = formatPrice(price, rates);

    return {
      ...ticket,
      id: uuid(),
      arrival_date: formattedArrivalDate,
      departure_date: formattedDepartureDate,
      stops: formattedStops,
      prices: formattedPrice,
    };
  };

  const formatTickets = (rates, tickets) => {
    const locale = 'ru-ru';

    return tickets
      .map(ticket => formatTicketFields(ticket, locale, rates));

  };

export default {
  name: 'TicketsList',

  props: {
    rawTickets: Array,
    currencyRates: Object,
    cur: String,
    stops: Object,
  },

  data() {
    const currency = this.cur;
    const rates = this.currencyRates;
    const sortedTickets = [ ...this.rawTickets]
      .sort((a, b) => (a.price - b.price));
    const tickets = formatTickets(rates, sortedTickets);

    let stop = this.stops;

    return {
      tickets,
      currency,
      stop,
    };
  },

  components: {
    TicketCard,
  },

  methods: {
    beforeEnter: function (element) {
      element.style.opacity = 0
      // element.style.height = 0
      element.style.marginBottom = 0
    },
    enter: function (element, done) {
      var delay = element.dataset.id * 150
      setTimeout(function () {
        Velocity(
          element,
          { opacity: 1, marginBottom: '20px' },
          { complete: done }
        )
      }, delay)
    },
    leave: function (element, done) {
      var delay = element.dataset.id * 150
      setTimeout(function () {
        Velocity(
          element,
          { opacity: 0, marginBottom: 0 },
          { complete: done }
        )
      }, delay)
    },
  },

  computed: {
    filteredTickets() {
      const pronounce = {
        0: 'Без пересадок',
        1: '1 пересадка',
        2: '2 пересадки',
        3: '3 пересадки',
      };

      const stops = {};

      for (const key in this.stops) {
        stops[pronounce[key]] = this.stops[key];
      }

      const filteredTickets = this.tickets.filter(ticket => {
        for (const key in stops) {
          if (ticket.stops === key && stops[key]) {
            return true;
          }
        }

        return false;
      });

      return filteredTickets;
    }
  },

  watch: { 
    cur(newVal) {
      this.currency = newVal;
    },
  },
}
</script>

<style scoped>

</style>
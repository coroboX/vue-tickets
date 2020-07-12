<template>
  <section
    class="tickets"
  >
    <TicketCard
      v-for="ticket in tickets"
      v-bind:key="ticket.id"
      v-bind:ticket="ticket"
      v-bind:currency="currency"
    />
  </section>
</template>

<script>
import TicketCard from './TicketCard.vue'
import {v4 as uuid} from 'uuid';

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

export default {
  name: 'TicketsList',

  props: {
    rawTickets: Array,
    currencyRates: Object,
    cur: String,
  },

  data() {
    const locale = 'ru-ru';
    const rates = this.currencyRates;
    const tickets = this.rawTickets
      .map(ticket => formatTicketFields(ticket, locale, rates));
    const currency = this.cur;

    return {
      tickets,
      currency,
    };
  },

  components: {
    TicketCard,
  },

  watch: { 
    cur(newVal) {
      this.currency = newVal;
    }
  },
}
</script>

<style scoped>

</style>
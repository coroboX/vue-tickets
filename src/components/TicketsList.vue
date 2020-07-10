<template>
  <section
    class="tickets"
  >
    <TicketCard
      v-for="ticket in tickets"
      v-bind:key="ticket.id"
      v-bind:ticket="ticket"
    />
  </section>
</template>

<script>
import TicketCard from './TicketCard.vue'
import {v4 as uuid} from 'uuid';

export default {
  name: 'TicketsList',
  props: ["tks"],
  data() {
    return {
      tickets: [],
    }
  },
  components: {
    TicketCard,
  },
  created() {
    const { tickets: rawTks } = this.tks;
    const prepTks = rawTks.map(ticket => ({
      ...ticket,
      id: uuid(),
    }))
    this.tickets = prepTks;
  }
}
</script>

<style scoped>

</style>
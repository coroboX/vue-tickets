<template>
  <div id="app">
    <ticket-card></ticket-card>
  </div>
</template>

<script>
import TicketCard from './components/TicketCard.vue'

export default {
  name: 'App',
  components: {
    TicketCard,
  }
}
</script>

<style>
  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  html {
    background-color: #eceff1;
  }
</style>

<template>
  <div id="app">
    <img
      src="./assets/logo.svg"
      alt="logo that displays white plane on blue circle"
    >
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
    font-family: Arial, Helvetica, sans-serif;
    font-size: 16px;
    background-color: #eceff1;
  }
</style>

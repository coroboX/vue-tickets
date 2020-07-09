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
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
</style>

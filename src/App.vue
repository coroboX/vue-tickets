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
      <FilterCard />

      <TicketsList
        v-bind:tks="tks"
      />
    </main>
  </div>
</template>

<script>
import FilterCard from './components/FilterCard.vue';
import TicketsList from './components/TicketsList.vue';
import tickets from './api/tickets'

export default {
  name: 'App',
  components: {
    FilterCard,
    TicketsList,
  },
  data() {
    return {
      tks: tickets,
    }
  },
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

    box-shadow: 0 0 0 1px rgb(248, 130, 130);
  }
</style>

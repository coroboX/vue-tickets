<template>
  <div class="filter">
    <div
      name="filter"
      class="filter-form"
    >
      <p class="filter-form__description">
        Валюта
      </p>
      <div class="radio-group">
        <label class="radio-label">
          <input
            type="radio"
            name="radio"
            value="RUB"
            v-model="currency"
            v-on:change="$emit('currency-changed', currency)"
          >
          <span class="radiomark">Rub</span>
        </label>
        <label class="radio-label">
          <input
            type="radio"
            name="radio"
            value="USD"
            v-model="currency"
            v-on:change="$emit('currency-changed', currency)"
          >
          <span class="radiomark">Usd</span>
        </label>
        <label class="radio-label">
          <input
            type="radio"
            name="radio"
            value="EUR"
            v-model="currency"
            v-on:change="$emit('currency-changed', currency)"
          >
          <span class="radiomark">Eur</span>
        </label>
      </div>
      
      <p class="filter-form__description">
        Количество пересадок
      </p>
      <div
        class="check-group"
        v-on:change="$emit('stops-changed', stops)"
      >
        <label class="checkbox-label">Все
          <input
            type="checkbox"
            name="all"
            v-model="stops['all']"
            v-on:change="handleAll"
          >
          <span class="checkmark checkmark--all"></span>
        </label>
        <label class="checkbox-label">Без пересадок
          <input
            type="checkbox"
            v-model="stops[0]"
            v-on:change="handleStops(0)"
          >
          <span class="checkmark"></span>
          <button
            class="checkbox-label__only"
            v-on:click="$emit('only', 0)"
          >
            Только
          </button>
        </label>
        <label class="checkbox-label">1 пересадка
          <input
            type="checkbox"
            v-model="stops[1]"
            v-on:change="handleStops(1)"
          >
          <span class="checkmark"></span>
          <button
            class="checkbox-label__only"
            v-on:click="$emit('only', 1)"
          >
            Только
          </button>
        </label>
        <label class="checkbox-label">2 пересадки
          <input
            type="checkbox"
            v-model="stops[2]"
            v-on:change="handleStops(2)"
          >
          <span class="checkmark"></span>
          <button
            class="checkbox-label__only"
            v-on:click="$emit('only', 2)"
          >
            Только
          </button>
        </label>
        <label class="checkbox-label">3 пересадки
          <input
            type="checkbox"
            v-model="stops[3]"
            v-on:change="handleStops(3)"
          >
          <span class="checkmark"></span>
          <button
            class="checkbox-label__only"
            v-on:click="$emit('only', 3)"
          >
            Только
          </button>
        </label>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'FilterCard',

  props: {
    filter: Object,
  },

  data() {
    const {
      currentCurrency,
      stops,
    } = this.filter;

    return {
      currency: currentCurrency,
      stops,
    };
  },

  methods: {
    handleAll() {
      const { all } = this.stops;

      if (all) {
        for (const key in this.stops) {
          this.stops[key] = true;
        }
      }
    },

    handleStops(val) {
      if (!this.stops[val]) {
        this.stops.all = false;
      }
    },
  },

  mounted () {
    this.currency = this.filter.currency;
  },
}
</script>

<style lang="scss">
  .filter {
    width: 232px;
    height: 332px;
    padding: 18px 0;
    margin-bottom: 20px;

    position: sticky;
    top: 20px;

    border-radius: 5px;
    background: #FFFFFF;
    box-shadow: 0px 1px 4px rgba(91, 137, 164, 0.25);

    @media (max-width: 840px) {
      position: static;
    }

  }

  .filter-form {
    font-size: 12px;
    letter-spacing: 0.5px;
    font-weight: 600;
    color: #4a4a4a;

    &__description {
      text-transform: uppercase;
      margin-bottom: 11px;
      padding-left: 15px;
    }
  }
  
  .checkbox-label {
    letter-spacing: 0;
    display: block;
    position: relative;
    padding-left: 45px;

    cursor: pointer;
    font-size: 13px;
    font-weight: normal;
    line-height: 35px;
    user-select: none;

    &__only {
      display: none;
      height: 100%;
      position: absolute;
      right: 15px;

      color: #2196f3;
      font-family: inherit;
      font-size: 11px;
      font-weight: 600;
      text-transform: uppercase;

      cursor: inherit;
      background-color: #f1fcff;
      outline: none;
      border: none;
    }

    &:hover {
      background-color: #f1fcff;
    }
  }

  .checkbox-label input {
    position: absolute;
    opacity: 0;
    cursor: pointer;
    height: 0;
    width: 0;
  }

  .checkmark {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    left: 15px;
    height: 19px;
    width: 19px;

    border-radius: 3px;
    border: 1px solid #d2d5d6;
    transition: box-shadow 0.25s ease;
  }

  .checkmark--all {
    height: 21px;
    width: 21px;
    left: 14px;

  }
  .checkbox-label:hover input ~ .checkmark {
      box-shadow: 0 0 2px 0px black;
  }

  .checkbox-label input:checked ~ .checkmark {
    border-color: #2196f3;
  }

  .checkmark:after {
    content: "";
    position: absolute;
    display: none;
  }

  .checkbox-label input:checked ~ .checkmark:after {
    display: block;
  }

  .checkbox-label:hover input:checked ~ .checkbox-label__only {
    display: inline;
  }

  .checkbox-label__only:focus,
  .checkbox-label__only:hover {
    display: inline;
    font-weight: 600;
    text-decoration: underline;
  }

  .checkmark:after {
    left: 6px;
    top: 3.5px;
    width: 3px;
    height: 6px;
    border: solid #2196f3;
    border-width: 0 2px 2px 0;
    transform: rotate(45deg);
  }

  .checkmark--all:after {
    left: 7px;
    top: 4px;
  }

  .radio-group {
    margin-bottom: 33px;
    height: 40px;
    padding: 0 15px;
  }

  .radio-label {
    position: relative;
    width: calc(33% + 1.5px);
    height: 100%;
    display: inline-block;
    cursor: pointer;
    margin-right: -1px;

    text-align: center;
    font-size: 22px;
    text-transform: uppercase;
    user-select: none;
  }

  .radio-label:first-child .radiomark {
    border-radius: 5px 0 0 5px;
  }

  .radio-label:last-child .radiomark {
    border-radius: 0 5px 5px 0;
  }

  .radio-label input {
    opacity: 0;
    cursor: pointer;
  }

  .radiomark {
    font-size: 12px;
    padding-top: 12px;
    font-weight: 600;
    color: #2196F3;

    position: absolute;
    top: 0;
    left: 0;
    height: 100%;
    width: 100%;
    background-color: #fff;
    box-shadow: inset 0 0 0 1px #d2d5d6;
    transition: background-color 0.25s ease,
      color 0.25s ease, box-shadow 0.25s ease;
  }

  .radio-label:hover input ~ .radiomark {
    background-color: #f2fcff;
    box-shadow: inset 0 0 0 1px #64b5f5;
    z-index: 2;
  }

  .radio-label input:checked ~ .radiomark {
    color: #fff;
    background-color: #2196f3;
    box-shadow: inset 0 0 0 1px #2196f3;
    z-index: 1;
  }
</style>

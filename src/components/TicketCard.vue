<template>
  <div class="ticket">
    <div class="ticket__action">
      <div
        :class="'ticket__carrier-logo ' + carrier"
      ></div>
      <button class="button ticket__button">
        <p class="button__description">
          Купить
        </p>
        <p class="button__price">
          за {{price}}
        </p>
      </button>
    </div>
    <div class="ticket__descriptions">
      <div class="ticket__departure">
        <p class="ticket__time ticket__time--departure">
          {{departure_time}}
        </p>
        <p class="ticket__place ticket__place--departure">
          {{origin}}, {{origin_name}}
        </p>
        <p class="ticket__date ticket__date--departure">
          {{departure_date}}
        </p>
      </div>
      <div class="ticket__arrival">
        <p class="ticket__time ticket__time--arrival">
          {{arrival_time}}
        </p>
        <p class="ticket__place ticket__place--arrival">
          {{destination_name}}, {{destination}}
        </p>
        <p class="ticket__date ticket__date--arrival">
          {{arrival_date}}
        </p>
      </div>
      <div class="ticket__stops">
        <span class="ticket__stops-text">
          {{stops}}
        </span>
        <div class="ticket__arrow"></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TicketCard',
  props: {
    ticket: Object,
    currency: String,
  },

  data() {
    const {
      origin,
      origin_name,
      destination,
      destination_name,
      departure_date,
      departure_time,
      arrival_date,
      arrival_time,
      carrier,
      stops,
      prices,
    } = this.ticket;

    return {
      origin,
      origin_name,
      destination,
      destination_name,
      departure_date,
      departure_time,
      arrival_date,
      arrival_time,
      carrier,
      stops,
      price: prices[this.currency],
      prices,
    };
  },

  watch: { 
    currency (newVal) {
      this.price = this.prices[newVal];
    }
  },
}
</script>

<style scoped lang="scss">
  .TK {
    background-image: url('./../assets/TK_logo.png');
  }
  .SU {
    background-image: url('./../assets/SU_logo.svg');
  }
  .S7 {
    background-image: url('./../assets/S7_logo.svg');
  }
  .BA {
    background-image: url('./../assets/BA_logo.svg');
  }
  
  .ticket {
    width: 566px;
    height: 161px;
    margin-bottom: 20px;
    display: flex;

    border-radius: 5px;
    background-color: #fff;

    &__action{
      width: 200px;
      height: 100%;
      padding: 25px 0;

      display: flex;
      flex-direction: column;
      justify-content: space-between;
      align-items: center;

      border-right: 1px solid #eceff1;
    }

    &__carrier-logo {
      width: 120px;
      height: 35px;
      background-size: contain;
      background-repeat: no-repeat;
      background-position: center;
    }

    &__button {
      color: #fff;
      font-family: "Open Sans", Helvetica, sans-serif;
      font-style: normal;
      font-weight: 600;
      font-size: 16px;
      line-height: 22px;

      width: 160px;
      height: 56px;
      text-align: center;

      background-color: #ff6d00;
      border: none;
      border-radius: 5px;
      transition: background-color 0.25s ease;
    }

    &__button:hover {
      background-color: #ff8124;
      cursor: pointer;
    }

    &__descriptions {
      color: #4a4a4a;

      position: relative;
      padding: 25px;

      flex-grow: 2;
      display: flex;
      justify-content: space-between;
    }

    &__time {
      font-size: 32px;
      line-height: 26px;
    }

    &__place {
      font-size: 12px;
      line-height: 18px;
      font-weight: 600;

      margin-top: 10px;
    }

    &__date {
      color: #8b9497;
      font-size: 12px;
      line-height: 18px;
    }

    &__arrival {
      text-align: right;
    }

    &__stops {
      width: 110px;

      position: absolute;
      top: 16px;
      left: 50%;

      transform: translateX(-50%);
    }

    &__stops-text {
      display: inline-block;
      width: 100%;

      color: #8b9497;
      font-size: 10px;
      margin-bottom: 7px;
      text-align: center;
      text-transform: uppercase;
    }

    &__arrow {
      width: 100%;
      height: 1px;

      background-color: #d2d5d6;
    }

    &__arrow::after {
      content: '';
      display: block;
      width: 15px;
      height: 13px;
      position: absolute;
      right: 0;
      transform: translateY(-6px);

      background: #fff no-repeat center;
      background-image: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='13' height='13' viewBox='0 0 13 13' fill='%23d2d5d6'><path d='M3.9 13H5.2L8.45 7.52632L12.025 7.52632C12.5645 7.52632 13 7.06789 13 6.5C13 5.93211 12.5645 5.47368 12.025 5.47368L8.45 5.47368L5.2 0L3.9 0L5.525 5.47368H1.95L0.975 4.10526L0 4.10526L0.65 6.5L0 8.89474H0.975L1.95 7.52632H5.525L3.9 13Z'/></svg>");
    }
  }

  @media (max-width: 600px) {
    .ticket {
      width: 95vw;
      min-width: 466px;
      margin-bottom: 10px;
      &__stops {
        top: 100px;
      }
    }
  }

  @media (max-width: 480px) {
    .ticket {
      flex-direction: column-reverse;
      height: auto;
      min-width: 232px;
      align-items: center;

      &__action {
        flex-direction: row-reverse;
        border: none;
        width: 90%;
      }

      &__button {
        margin-right: 20px;
      }

      &__descriptions {
        width: 80vw;
        min-width: 260px;
      }
    }
  }

</style>

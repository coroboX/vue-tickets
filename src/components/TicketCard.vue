<template>
  <div class="ticket">
    <div class="ticket__action">
      <logo class="ticket__saller-logo"></logo>
      <button class="button ticket__button">
        <p class="button__description">
          Купить
        </p>
        <p class="button__price">
          за 20 056 ₽
        </p>
      </button>
    </div>


  </div>
</template>

<script>
export default {

}
</script>

<style scoped lang="scss">
  .ticket {
    height: 161px;
    width: 566px;
    display: flex;

    border-radius: 5px;
    background-color: #fff;

    &__action{ 
      width: 200px;
      height: 100%; 
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
    }

    &__button:hover {
      background-color: #ff8124;
      cursor: pointer;
    }
  }

</style>
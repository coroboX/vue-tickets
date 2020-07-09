<template>
  <div class="ticket">
    <div class="ticket__action">
      <img
        class="ticket__carrier-logo"
        src="./../assets/turkish-airlines-logo-sm.png"
      />
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
    margin: 50px auto 0;
    display: flex;

    border-radius: 5px;
    background-color: #fff;
    border: 1px solid pink;

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
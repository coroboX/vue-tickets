<template>
  <div class="filter">
    <form
      name=filter
    >
      <div class="radio-group">
        <label class="radio-label">
          <input
            type="radio"
            name="radio"
            checked="checked"
          >
          <span class="radiomark">Rub</span>
        </label>
        <label class="radio-label">
          <input
            type="radio"
            name="radio"
          >
          <span class="radiomark">Usd</span>
        </label>
        <label class="radio-label">
          <input
            type="radio"
            name="radio"
          >
          <span class="radiomark">Eur</span>
        </label>
      </div>
      
      <hr>
      <label class="checkbox-label">Все
        <input
          type="checkbox"
        >
        <span class="checkmark checkmark--all"></span>
      </label>
      <label class="checkbox-label">Без пересадок
        <input
          type="checkbox"
        >
        <span class="checkmark"></span>
      </label>
      <label class="checkbox-label">1 пересадка
        <input
          type="checkbox"
        >
        <span class="checkmark"></span>
      </label>
      <label class="checkbox-label">2 пересадки
        <input
          type="checkbox"
          checked="checked"
        >
        <span class="checkmark"></span>
      </label>
      <label class="checkbox-label">3 пересадки
        <input
          type="checkbox"
          checked="checked"
        >
        <span class="checkmark"></span>
      </label>
    </form>
  </div>
</template>

<script>
export default {
  name: 'filter-card',
}
</script>

<style>
  .filter {
    width: 232px;
    height: 332px;
    padding: 15px;

    border-radius: 5px;
    background: #FFFFFF;
    box-shadow: 0px 1px 4px rgba(91, 137, 164, 0.25);
    border: 1px solid rgb(201, 192, 255);
  }

  .checkbox-label {
    display: block;
    position: relative;
    padding-left: 35px;
    cursor: pointer;
    font-size: 13px;
    line-height: 35px;
    user-select: none;
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
    left: 0;
    height: 19px;
    width: 19px;

    border-radius: 3px;
    border: 1px solid #d2d5d6;
  }

  .checkmark--all {
    height: 21px;
    width: 21px;
  }
  .checkbox-label:hover input ~ .checkmark {
      box-shadow: 0 0 2px 0px black;
  }

  .checkbox-label input:checked ~ .checkmark {
    border-color: #2196F3;
  }

  .checkmark:after {
    content: "";
    position: absolute;
    display: none;
  }

  .checkbox-label input:checked ~ .checkmark:after {
    display: block;
  }

  .checkbox-label .checkmark:after {
    left: 6.5px;
    top: 4px;
    width: 3px;
    height: 6px;
    border: solid #2196f3;
    border-width: 0 2px 2px 0;
    transform: rotate(45deg);
  }

  .radio-group {
    width: 202px;
    height: 40px;
  }

  .radio-label {
    position: relative;
    width: 33%;
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
    position: absolute;
    top: 0;
    left: 0;
    height: 100%;
    width: 100%;
    background-color: #eee;
    box-shadow: inset 0 0 0 1px #d2d5d6;
  }

  .radio-label:hover input ~ .radiomark {
    background-color: #f2fcff;
    box-shadow: inset 0 0 0 1px #64b5f5;
    z-index: 2;
  }

  .radio-label input:checked ~ .radiomark {
    background-color: #2196f3;
    box-shadow: inset 0 0 0 1px #2196f3;
    z-index: 1;
  }
</style>

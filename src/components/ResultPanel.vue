<template>
  <div class="result-panel">
    <div class="card">
      <div class="card__top">
        <img class="card__top__img" v-bind:src="country.selectedCountry.flag" alt="flag" width="32" />
        <p class="card__top__text">{{ country.selectedCountry.Country }}</p>
      </div>

      <div class="card__item">
        <p class="label-new">new</p>
        <p class="label-total">total</p>
      </div>

      <div class="card__item">
        <div class="card__item__category">
          <img class="card__item__img" src="../assets/virus-icon.svg" alt="virus-icon" />
          <p class="card__item__text">cases</p>
        </div>
        <p class="card__item__count">+{{ country.selectedCountry.NewConfirmed }}</p>
        <p class="card__item__count">{{ country.selectedCountry.TotalConfirmed }}</p>
      </div>

      <div class="card__item">
        <div class="card__item__category">
          <img class="card__item__img" src="../assets/recover-icon.svg" alt="virus-icon" />
          <p class="card__item__text">recovered</p>
        </div>
        <p class="card__item__count">+{{ country.selectedCountry.NewRecovered }}</p>
        <p class="card__item__count">{{ country.selectedCountry.TotalRecovered }}</p>
      </div>

      <div class="card__item">
        <div class="card__item__category">
          <img class="card__item__img" src="../assets/death-icon.svg" alt="virus-icon" />
          <p class="card__item__text">deaths</p>
        </div>
        <p class="card__item__count">+{{ country.selectedCountry.NewDeaths }}</p>
        <p class="card__item__count">{{ country.selectedCountry.TotalDeaths }}</p>
      </div>

      <footer class="card__footer">
        <p>last updated: {{ country.lastUpdated.date }} {{ country.lastUpdated.time }}</p>
      </footer>
    </div>
  </div>
</template>

<script>
export default {
  name: "resultPanel",
  props: {
    country: Object
  }
};
</script>

<style lang="scss" scoped>
$font-dark: #282828;
$font-light: #626262;
$font-lighter: #aba9a9;
$font-regular: #474646;

.result-panel {
  padding: 0 1rem;
}

.card {
  border: 1px solid #98d3ab;
  margin: 1rem auto;
  border-radius: 10px;
  box-shadow: 0px 2px 5px rgba(14, 241, 91, 0.15);

  &__top {
    text-align: center;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 0.8rem 0.4rem;
    color: $font-dark;
    border-bottom: 1px solid #d9efe0;

    &__img {
      width: 32px;
    }

    &__text {
      font-weight: 500;
      text-transform: uppercase;
    }
  }

  &__item {
    display: grid;
    grid-template-columns: 2fr repeat(2, 1fr);
    padding: 0.6rem;
    color: $font-regular;

    &__category {
      display: flex;
      color: $font-light;
      text-transform: capitalize;

      &__text {
        margin-left: 0.2rem;
      }
    }

    &__count {
      font-weight: 500;
      text-align: center;
    }
  }

  &__footer {
    font-weight: 300;
    color: $font-lighter;
    font-size: 0.8rem;
    text-transform: capitalize;
    text-align: center;
    padding: 0.6rem 0;
  }

  .label-new,
  .label-total {
    text-align: center;
    color: $font-light;
  }

  .label-new {
    grid-column: 2;
  }

  .label-total {
    grid-column: 3;
  }
}
</style>

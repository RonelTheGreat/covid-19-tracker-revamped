<template>
  <div class="search-container" ref="searchContainer">
    <form class="search" v-on:submit.stop>
      <input
        class="search__input"
        v-model.trim="countryName"
        type="text"
        placeholder="Search Country"
      />
      <img class="search__img" src="../assets/search-icon.svg" alt="search-icon" />
    </form>

    <!-- SEARCH RESULT CONTAINER -->
    <ul class="result" v-show="searchResult.length >= 1">
      <h3 class="result__label">
        Found
        <span>{{searchResult.length}} {{searchResult.length > 1 ? 'matches': 'match'}}</span>
      </h3>
      <li
        class="result__item"
        v-on:click="getCountry(country.code)"
        v-for="(country, index) in searchResult"
        v-bind:key="index"
      >{{country.name}}</li>
    </ul>
    <!-- END SEARCH RESULT CONTAINER -->
  </div>
</template>

<script>
export default {
  name: "SearchBar",
  props: {
    countries: Array
  },
  data() {
    return {
      countryName: "",
      searchResult: [],
      isFocused: true
    };
  },

  watch: {
    /**
     * filter according to typed characters
     * from the input
     */
    countryName: function() {
      if (this.countryName.length >= 1) {
        // iterate through countries
        // and filter according to input value
        const result = this.countries
          .map(country => {
            return {
              name: country.Country,
              slug: country.Slug,
              code: country.CountryCode
            };
          })
          .filter(country => {
            return country.name
              .toLowerCase()
              .startsWith(this.countryName.toLowerCase());
          });

        this.searchResult = result;
      } else {
        this.searchResult = [];
      }
    }
  },
  methods: {
    /**
     * this will emit an event
     * once a country is selected
     * @param {string} countryCode - the code obtained from filtered data
     */
    getCountry(countryCode) {
      this.$emit("get-country", countryCode);
      this.searchResult = [];
      this.countryName = "";
    }
  }
};
</script>

<style lang="scss" scoped>
$font-dark: #282828;
$font-light: #626262;
$font-lighter: #aba9a9;
$font-regular: #474646;

.search-container {
  padding: 0 1rem;
  position: relative;
  margin: 1rem auto;
}

.focus {
  border: 2px solid #009432;
}

.search {
  border: 2px solid #a5d8b5;
  border-radius: 10px;
  display: flex;
  padding: 0.4rem 0.7rem;
  margin: 0 auto 0.5rem;

  &__input {
    width: 100%;
    font-size: inherit;
    border: none;
    padding-top: 0.1rem;
    color: $font-regular;
    background: transparent;

    &:focus {
      outline: none;
    }

    &::placeholder {
      color: #ccead6;
      padding: 0;
      margin: 0;
    }
  }
}

.result {
  box-shadow: 0px 1px 5px #cde6d6;
  list-style-type: none;
  background: #fff;
  border-bottom-right-radius: 10px;
  border-bottom-left-radius: 10px;
  text-align: center;
  max-height: 12rem;
  overflow-y: scroll;

  &__label,
  &__item {
    text-align: left;
    padding: 0.6rem 1rem;
  }

  &__label {
    font-weight: 300;
    font-size: 0.85rem;
    color: $font-lighter;
    position: sticky;
    top: 0;
    background: #fff;
    border-bottom: 1px solid #98d3ab;
  }

  &__item {
    color: $font-regular;
    text-transform: capitalize;

    &:hover {
      background: #009432;
      color: #fff;
      cursor: pointer;
    }
  }

  &__img {
    position: sticky;
    bottom: 0;
  }
}
</style>

<template>
  <div id="app">
    <app-banner />
    <search-bar v-bind:countries="countries" v-on:get-country="getCountry" />
    <result-panel v-if="hasResult" v-bind:country="{selectedCountry, lastUpdated}" />
    <global-dashboard v-if="!isFetching" v-bind:global="global" />
    <app-footer />
  </div>
</template>

<script>
import AppBanner from "./components/AppBanner.vue";
import SearchBar from "./components/SearchBar.vue";
import ResultPanel from "./components/ResultPanel.vue";
import GlobalDashboard from "./components/GlobalDashboard.vue";
import AppFooter from "./components/AppFooter.vue";

export default {
  name: "App",
  components: {
    AppBanner,
    SearchBar,
    ResultPanel,
    GlobalDashboard,
    AppFooter
  },
  data() {
    return {
      isFetching: false,
      isReady: false,
      countries: [],
      global: {},
      lastUpdated: {
        time: "",
        date: ""
      },
      hasResult: false,
      selectedCountry: {},
      strMonths: [
        "Jan.",
        "Feb.",
        "March",
        "April",
        "May",
        "June",
        "July",
        "Aug.",
        "Sept.",
        "Oct.",
        "Nov.",
        "Dec."
      ]
    };
  },
  mounted() {
    this.fetchAvailableCountries();
  },
  methods: {
    /**
     * get the searched country
     * from the list of available countries
     * @param {string} received - received from search input
     */
    getCountry(received) {
      // filter available countries
      // according to search input
      let selected = this.countries.filter(
        country => country.CountryCode === received
      )[0];

      // format each data from e.g. 5000 to 5, 000
      // or add a comma for better visualization of number
      selected.NewConfirmed = this.formatNumber(selected.NewConfirmed);
      selected.TotalConfirmed = this.formatNumber(selected.TotalConfirmed);
      selected.NewRecovered = this.formatNumber(selected.NewRecovered);
      selected.TotalRecovered = this.formatNumber(selected.TotalRecovered);
      selected.NewDeaths = this.formatNumber(selected.NewDeaths);
      selected.TotalDeaths = this.formatNumber(selected.TotalDeaths);

      // set selected country and
      // fetch appropriate flag
      this.selectedCountry = selected;
      this.selectedCountry.flag = `https://www.countryflags.io/${this.selectedCountry.CountryCode.toLowerCase()}/flat/32.png`;
      this.selectedCountry.date = this.lastUpdated.date;
      this.selectedCountry.time = this.lastUpdated.time;

      // show result
      this.hasResult = true;
    },
    /**
     * fetch all available countries through api
     */
    fetchAvailableCountries() {
      this.isFetching = true;
      fetch("https://api.covid19api.com/summary", { method: "GET" })
        .then(res => {
          return res.json();
        })
        .then(data => {
          // set countries data
          // to all countries fetched from the api
          this.countries = data.Countries;
          this.global = data.Global;

          let {
            NewConfirmed,
            TotalConfirmed,
            NewRecovered,
            TotalRecovered,
            NewDeaths,
            TotalDeaths
          } = this.global;

          // format each data from e.g. 5000 to 5, 000
          // or add a comma for better visualization of number
          this.global.NewConfirmed = this.formatNumber(NewConfirmed);
          this.global.TotalConfirmed = this.formatNumber(TotalConfirmed);
          this.global.NewRecovered = this.formatNumber(NewRecovered);
          this.global.TotalRecovered = this.formatNumber(TotalRecovered);
          this.global.NewDeaths = this.formatNumber(NewDeaths);
          this.global.TotalDeaths = this.formatNumber(TotalDeaths);
          const refDate = data.Date;

          // format date and time
          const date = new Date(refDate);
          const lastUpdatedDate = `${
            this.strMonths[date.getMonth()]
          } ${date.getDate()}, ${date.getFullYear()}`;

          const time = this.formatTime(date);

          // set time and date DATA
          this.lastUpdated.date = lastUpdatedDate;
          this.lastUpdated.time = time;

          this.getCountry("PH");
          this.isFetching = false;
        });
    },

    /**
     * format number i.e. add a comma separator
     * for better visualization
     * @param {number} num - number to format
     */
    formatNumber(num) {
      return num.toString().replace(/(\d)(?=(\d{3})+(?!\d))/g, "$1, ");
    },
    formatTime(date) {
      // get hour and minute
      let hours = date.getHours();
      let minutes = date.getMinutes();

      // get time of the day, PM or AM
      // if hour is greater than 12, then it is PM else it is AM
      let ampm = hours >= 12 ? "PM" : "AM";

      // get hour
      hours = hours % 12;
      hours = hours ? hours : 12;

      // if minute less than 10, add 0 to the front (e.g 09, 08 etc.)
      minutes = `${minutes < 10 ? "0" + minutes : minutes}`;

      // store the formatted time
      let time = `${hours}:${minutes} ${ampm}`;

      return time;
    }
  }
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-size: 18px;
  font-family: "Roboto", sans-serif;
  font-weight: 400;
}

@media (min-width: 768px) {
  #app {
    height: 100vh;
  }
  .result-panel {
    width: 60%;
    margin: 0 auto;
  }

  .worldwide {
    .w-top {
      padding-left: 3rem;
    }
  }

  .search-container {
    width: 60%;
    margin: 2rem auto;
  }
}

@media (min-width: 1024px) {
  #app {
    height: 100vh;
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    grid-template-rows: 15% 3rem 2fr 4rem;
    column-gap: 5%;
    padding: 2rem;
    grid-template-areas:
      "b ."
      "sc w"
      "rp w"
      "f g";

    .banner {
      grid-area: b;
      align-self: center;
      background-image: none;

      &__text {
        color: #009432;
      }
    }

    .search-container {
      margin: 0 auto;
      grid-area: sc;
      width: 70%;
    }

    .result-panel {
      grid-area: rp;
      margin: 1.8rem auto;
      min-width: 70%;
    }

    .worldwide {
      grid-area: w;
      align-self: flex-start;
      margin: 0 5%;
      background-image: none;

      .w-top {
        padding: 0.4rem 0 0;
      }

      .mcard-container {
        justify-content: flex-start;
        padding: 0;
        .mcard {
          width: 60%;
        }
      }
    }

    footer {
      grid-area: f;

      p {
        display: none;
      }
      img {
        max-width: 32px;
      }
    }
  }
}
</style>

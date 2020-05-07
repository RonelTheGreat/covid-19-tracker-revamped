<template>
  <div class="banner">
    <h2 class="banner__text">covid-19 tracker</h2>
  </div>
</template>

<script>
export default {
  name: "AppBanner"
};
</script>

<style lang="scss" scoped>
.banner {
  background-image: url("../assets/top-background.svg");
  background-repeat: no-repeat;
  background-position: top;
  background-size: 100%;
  height: 7rem;

  &__text {
    height: 6rem;
    display: grid;
    place-content: center;
    text-transform: uppercase;
    color: #fff;
    font-weight: 500;
    text-align: center;
  }
}
</style>
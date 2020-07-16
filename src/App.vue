<template>
  <div id="app">
    <img src="./assets/logo.png" style="max-height:150px;width:auto;border-radius:7px;">
    <TripConsumptionCalculator/>
  </div>
</template>

<script>
import TripConsumptionCalculator from "./components/TripConsumptionCalculator";

export default {
  name: "App",
  components: {
    TripConsumptionCalculator
  }
};
</script>

<style>
body {
  margin: 0;
  padding: 0;
}
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

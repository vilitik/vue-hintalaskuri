<template>
  <div v-show="num">
    <label v-if="label">{{ label }}</label>
    <p>{{ num }}</p>
  </div>
</template>

<script>
export default {
  props: {
    label: {
      default: null
    },
    num: {
      default: "15"
    }
  }
};
</script>

<style scoped>
label {
  display: block;
  text-align: left;
  color: #00402C;
  font-family: "Inter", sans-serif;
  font-weight: 700;
  font-size: 16px;
}
p {
  margin: 0;
  color: #00C388;
  font-family: "Inter", sans-serif;
  font-weight: 800;
  font-size: 35px;
}
</style>

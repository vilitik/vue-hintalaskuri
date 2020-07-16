<template>
  <div class="stack" :style="style">
    <slot/>
  </div>
</template>

<script>
export default {
  props: {
    color: {
      default: null
    }
  },
  computed: {
    style() {
      return this.color ? `background: ${this.color}` : null;
    }
  }
};
</script>

<style scoped>
.stack {
  display: flex;
  flex-flow: column wrap;
  max-width: 500px;
  margin: 25px auto 50px auto;
  border-radius: 7px
}
.stack > * {
  margin: 10px;
}
</style>

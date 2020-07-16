<template>
  <div class="stack">
    <slot/>
  </div>
</template>

<style scoped>
.stack {
  display: flex;
  flex-flow: row;
  max-width: 500px;
  width: 100%;
  margin: 0 auto 10px auto;
  justify-content: space-between;
  box-sizing: border-box;
}
.stack > *:not(.stack) {
  margin: 10px;
}
</style>

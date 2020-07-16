<template>
  <div>
    <label>{{ label }}</label>
    <select @input="handle($event.target.value)" :value="value">
      <option v-for="(i, ind) in items" :key="ind" :value="i">{{ i }}</option>
    </select>
  </div>
</template>

<script>
export default {
  props: {
    value: {
      default: null
    },
    items: {
      default: []
    },
    label: {
      default: null
    }
  },
  methods: {
    handle(val) {
      return void this.$emit("input", val);
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
select {
  padding: 10px 15px;
  width: 100%;
  outline: 0px transparent !important;
  border: #00C388 3px solid;
  color: #00402C;
  border-radius: 7px;

  box-sizing: border-box;
  box-shadow: 0px 1px 4px rgba(0, 0, 0, 0.25),
    inset 0px 0px 15px rgba(0, 0, 0, 0.35);

  font-family: "Inter", sans-serif;
  font-weight: 500;
  font-size: 20px;

  transition: border-color 0.25s ease;
}
select:focus {
  border-color: #00402C;
}
</style>

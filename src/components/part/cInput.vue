<template>
  <div>
    <label>{{ label }}</label>
    <input
      @input="handle($event.target.value)"
      @blur="blur"
      :value="suffix && value ? value + suffix : value"
      type="text"
      @focus="$event.target.select()"
      :placeholder="placeholder"
    >
  </div>
</template>

<script>
export default {
  props: {
    value: {
      default: null
    },
    label: {
      default: null
    },
    placeholder: {
      default: null
    },
    validate: {
      default: null
    },
    suffix: {
      default: null
    }
  },
  methods: {
    handle(inp) {
      let val = inp;
      if (this.suffix) val = val.replace(this.suffix, "");
      if (!this.validate) return void this.$emit("input", val);
      switch (this.validate) {
        case "decimal":
          val = val.replace(",", ".");
          if (/^\d+(\.\d+(e\d+)?)?$/.test(val) || !val)
            return void this.$emit("input", val);
          console.log("cInput vahvistus ei onnistunut: NOT DECIMAL " + val);
          break;

        default:
          break;
      }
    },
    blur() {
      this.$emit("blur");
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
input {
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
input:focus {
  border-color: #00402C;
}
input::selection {
  background: #00402C;
  color: #00C388;
}
</style>

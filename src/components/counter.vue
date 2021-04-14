<template>
  <div @click="focused" :class="['counter', { focus: focus }]">
    <input
      ref="input"
      @input="input"
      @focus="focus = true"
      @blur="focus = false"
      type="number"
      v-model="inputValue"
    />
  </div>
</template>

<script>
export default {
  name: "counter",
  props: {
    max: {
      type: Number,
      default: 99,
    },
    min: {
      type: Number,
      default: 0,
    },
    value: {
      type: Number,
      default: null,
    },
  },
  data() {
    return {
      focus: false,
      inputValue: "",
    };
  },
  mounted() {
    this.inputValue = this.value;
  },
  methods: {
    focused() {
      this.$refs.input.focus();
    },
    input() {
      let value = Number(this.inputValue);
      value = value > this.max ? this.max : value;
      value = value < this.min || value === 0 ? null : value;
      this.inputValue = value;
      this.$emit("input", value);
    },
  },
  watch: {
    value(val) {
      this.inputValue = val;
    },
  },
};
</script>

<style scoped lang="scss">
.counter {
  position: relative;
  background-color: #fff;
  padding: 8px 10px;
  border-radius: 4px;
  border: 2px solid #ddd;
  cursor: text;

  &.focus {
    border-color: #319ae6;
  }

  input {
    width: 100%;
    border: none;
    background-color: transparent;
    outline: none;
    -moz-appearance: textfield;

    &::-webkit-outer-spin-button,
    &::-webkit-inner-spin-button {
      -webkit-appearance: none;
    }

    &::placeholder {
      color: #979797;
    }
  }
}
</style>

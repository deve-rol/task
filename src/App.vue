<template>
  <div id="app">
    <div class="column">
      <counter :min="1" v-model="numberOne" />
      <div class="line"></div>
      <counter :min="1" v-model="numberTwo" />
    </div>

    <div class="operator">+</div>

    <div class="column">
      <counter :min="1" v-model="numberThree" />
      <div class="line"></div>
      <counter :min="1" v-model="numberFour" />
    </div>

    <div class="equal">=</div>

    <div class="sum">{{ sum }}</div>
  </div>
</template>

<script>
import counter from "./components/counter";
export default {
  name: "App",
  components: {
    counter,
  },
  data() {
    return {
      numberOne: null,
      numberTwo: null,
      numberThree: null,
      numberFour: null,
    };
  },
  computed: {
    sum() {
      if (
        this.numberOne === null ||
        this.numberTwo === null ||
        this.numberThree === null ||
        this.numberFour === null
      )
        return;

      const sum = this.calcSum(
        this.numberOne / this.numberTwo,
        this.numberThree / this.numberFour
      );

      return sum.toFixed(2);
    },
  },
  methods: {
    calcSum(a, b) {
      const max = Math.max(a, b);
      const power = Math.ceil(Math.log10(max + 1));
      const factor = 10 ** power;

      return (a * factor + b * factor) / factor;
    },
  },
};
</script>

<style lang="scss">
#app {
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 100vh;

  .column {
    width: 50px;

    .line {
      margin: 10px 0;
      height: 1px;
      background-color: #dddddd;
    }
  }

  .operator,
  .equal {
    margin: 0 10px;
  }
}
</style>

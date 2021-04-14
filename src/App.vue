<template>
  <div id="app">
    <div class="calc">
      <div v-for="(item, key) in models" :key="key" class="column-wrap">
        <div class="column">
          <div
            v-show="models.length > 2"
            @click="removeColumnByKey(key)"
            class="clear-icon"
          >
            <svg width="8" height="8" fill="#777" viewBox="0 0 241.171 241.171">
              <path
                id="Close"
                d="M138.138,120.754l99.118-98.576c4.752-4.704,4.752-12.319,0-17.011c-4.74-4.704-12.439-4.704-17.179,0 l-99.033,98.492L21.095,3.699c-4.74-4.752-12.439-4.752-17.179,0c-4.74,4.764-4.74,12.475,0,17.227l99.876,99.888L3.555,220.497 c-4.74,4.704-4.74,12.319,0,17.011c4.74,4.704,12.439,4.704,17.179,0l100.152-99.599l99.551,99.563 c4.74,4.752,12.439,4.752,17.179,0c4.74-4.764,4.74-12.475,0-17.227L138.138,120.754z"
              />
            </svg>
          </div>

          <counter :min="1" v-model="item[0]" />
          <div class="line"></div>
          <counter :min="1" v-model="item[1]" />
        </div>

        <div v-show="key !== models.length - 1" class="operator">+</div>
      </div>

      <div class="equal">=</div>

      <div class="sum">{{ sum }}</div>
    </div>

    <button @click="addColumn" :disabled="addColumnDisabled">
      add new element
    </button>
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
      models: [],
    };
  },
  computed: {
    sum() {
      const hasNull = this.models.flat(1).includes(null);
      if (hasNull) return "result";

      let res = 0;
      this.models.forEach((arr) => {
        res = this.calcSum(res, arr[0] / arr[1]);
      });

      return res.toFixed(2);
    },
    addColumnDisabled() {
      return this.models.length > 4;
    },
  },
  mounted() {
    this.addColumn();
    this.addColumn();
  },
  methods: {
    addColumn() {
      this.models.push([null, null]);
    },
    removeColumnByKey(key) {
      this.models = this.models.filter((arr, i) => i !== key);
    },
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
  flex-direction: column;
  min-height: 100vh;

  .calc {
    display: flex;
    align-items: center;
    margin-bottom: 10px;

    .column-wrap {
      display: flex;
      align-items: center;

      .column {
        position: relative;
        width: 50px;

        .clear-icon {
          position: absolute;
          top: 0;
          right: 0;
          display: flex;
          background-color: #fff;
          z-index: 1;
          opacity: 0;
          transition: all 0.2s;
          cursor: pointer;
          padding: 2px;

          svg {
            transition: all 0.2s;
          }
          &:hover svg {
            fill: #000;
          }
        }
        &:hover .clear-icon {
          opacity: 1;
        }

        .line {
          margin: 10px 0;
          height: 1px;
          background-color: #dddddd;
        }
      }

      .operator {
        margin: 0 10px;
      }
    }
    .equal {
      margin: 0 10px;
    }
  }
}
</style>

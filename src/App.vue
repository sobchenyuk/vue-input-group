<template>
  <div id="app">

    <InputGroup :maxlength="maxlength" :countInput="countInput" @v-input="vInput"/>

    <p class="result">Script result: <span class="result__input">{{ inputGroupText | join }}</span></p>

  </div>
</template>

<script>

import InputGroup from "@/components/InputGroup";
export default {
  name: 'app',

  components: {InputGroup},

  data() {
    return {
      countInput: 4,
      maxlength: 2,
      inputGroupText: []
    }
  },

  filters: {
    join(value) {
      if (!value) return ''
      return value.join('-')
    }
  },

  methods: {

    vInput(result) {
      this.inputGroupText = result;
    }

  }
}
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 20em;
    max-width: 1200px;
    margin-left: auto;
    margin-right: auto;
  }

  .result {
    margin-top: 1em;
    font-size: 50px;
    font-weight: 900;
  }
  @media (max-width: 768px) {
      .result {
          font-size: 40px;
      }

      .result__input {
          display: block;
      }
  }
</style>

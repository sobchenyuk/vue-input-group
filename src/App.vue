<template>
  <div id="app">

    <InputGroup :maxlength="maxlength" :countInput="countInput" />

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
</style>

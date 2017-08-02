<template>
  <div id="app">
    <hello></hello>
  </div>
</template>

<script>
import Hello from './components/Hello'

export default {
  name: 'app',
  components: {
    Hello
  }
}
</script>

<style>
*, *::after, *::before {
  box-sizing: inherit;
}

html {
  font-size: 14px !important;
  font-family: sans-serif;
  -webkit-text-size-adjust: 100%;
  box-sizing: border-box;
}
body {
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
  text-rendering: optimizeLegibility;
  font-family: "Helvetica Neue", "Helvetica Neue", Helvetica, Arial, "Hiragino Sans GB", "Microsoft Yahei", sans-serif;
  margin: 0;
  /*min-width: 1280px;*/
  /*min-height: 800px;*/
  /*overflow: scroll;*/
}
</style>

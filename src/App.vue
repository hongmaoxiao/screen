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
  font-family: Helvetica Neue,Helvetica,PingFang SC,Hiragino Sans GB,Microsoft YaHei,Arial,sans-serif;
  margin: 0;
}
</style>

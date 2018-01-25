<template>
  <div id="app">
      测试页面
  </div>
</template>

<script>
import Common from "common/js/common"
export default {
  name: "App",
  created() {
      Common.say();
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

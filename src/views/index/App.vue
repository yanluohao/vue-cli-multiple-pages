<template>
  <div id="app">
      23344555
    <HelloWorld/>
  </div>
</template>

<script>
import HelloWorld from "components/HelloWorld";
import Common from "common/js/common"

export default {
  name: "App",
  components: {
    HelloWorld
  },
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

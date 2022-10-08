<!--
 * @Author: zhengyunan
 * @Date: 2022-07-31 13:49:01
 * @FilePath: /magicknocks/src/App.vue
 * @Description: 模块介绍
-->
<template>
  <div id="app">
    <Magic />
  </div>
</template>

<script>
import Magic from "./components/echarts/index.vue";

export default {
  name: "App",
  components: {
    Magic,
  },
};
</script>

<style>
#app {
  /* font-family: Avenir, Helvetica, Arial, sans-serif; */
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}
</style>

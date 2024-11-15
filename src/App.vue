<template>
  <div id="app">
    <ConsoleInput />
  </div>
</template>

<script>
// 引入组件
import ConsoleInput from './components/ConsoleInput.vue';

export default {
  name: 'App',
  components: {
    ConsoleInput,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
}
</style>

<template>
  <MemoItem />
</template>

<script>
import MemoItem from './components/MemoItem.vue'
export default {
  name: 'App',
  components: { MemoItem }
}

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

ul {
  padding: 0;
  list-style: none;
}

</style>

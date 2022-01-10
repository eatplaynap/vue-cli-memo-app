<template>
  <h1>All Memos</h1>
  <ul>
    <li v-for="(memo, index) in memos" :key="index">{{ memo }}</li>
  </ul>
  <button>+</button>
  <form  @submit.prevent="addMemo">
    <label>Edit:</label>
    <input type="text" v-model="newMemo">
    <button type="submit">Add</button>
    <button>Delete</button>
  </form>

</template>

<script>

export default {
  name: 'App',
  components: {},
  data() {
    return {
      memos: [],
      newMemo: undefined
    }
  },
  methods: {
    addMemo()
    {
      this.memos.push(this.newMemo)
      this.newMemo = undefined
    }
  }
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

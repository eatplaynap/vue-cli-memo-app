<template>
  <h1>All Memos</h1>
  <ul>
    <li>Memo1</li>
    <li>Memo2</li>
    <li>Memo3</li>
  </ul>
  <button>+</button>
  <form>
    <label>Edit:</label>
    <input type="text">
    <span class="buttons">
      <button type="submit">Add</button>
      <button>Delete</button>
    </span>
  </form>

</template>

<script>

export default {
  name: 'App',
  components: {
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

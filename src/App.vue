<template>
  <h1>All Memos</h1>
  <ul>
    <li v-for="(memo, index) in memos" :key="index" @click="editMemo(index)">{{ memo }}</li>
  </ul>
  <button>+</button>
  <form  @submit.prevent="setMemo">
    <label>Edit:</label>
    <input type="text" v-model="newMemo">
    <button type="submit">Add</button>
  </form>
  <button @click="deleteMemo">Delete</button>

</template>

<script>

export default {
  name: 'App',
  components: {},
  data() {
    return {
      memos: [],
      newMemo: undefined,
      editIndex: null
    }
  },
  watch: {
    memos: {
      handler(memos) {
        localStorage.setItem('memos', JSON.stringify(this.memos))
      },
      deep: true
    }
  },
  mounted() {
    this.memos = JSON.parse(localStorage.getItem('memos')) || []
  },
  methods: {
    setMemo() {
      if(this.editIndex === null) {
        this.memos.push(this.newMemo)
      } else {
        this.memos.splice(this.editIndex, 1, this.newMemo)
      }
      this.cancel()
    },
    cancel() {
      this.newMemo = undefined
      this.editIndex = null
    },
    deleteMemo() {
      console.log('reached here')
    },
    editMemo(index){
      console.log('reached here')
      this.editIndex = index
      this.newMemo = this.memos[index]
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

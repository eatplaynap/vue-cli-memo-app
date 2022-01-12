<template>
  <h1>All Memos</h1>
  <ul>
    <li v-for="(memo, index) in memos" :key="memo.id" @click="editMemo(index)">{{ memo.title }}</li>
  </ul>
  <button @click="setMemo">+</button>
  <form  @submit.prevent="setMemo">
    <label>Edit:</label>
    <textarea v-model="newMemo"></textarea>
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
      editIndex: null,
      nextId: 0
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
      const memo = this.newMemo
      if(this.editIndex === null) {
        this.memos.push({
          id: this.nextId++,
          title: memo.split('\n')[0],
          content: memo
        })
        console.log(this.memos[0].id)
      } else {
        this.memos.splice(this.editIndex, 1, {
          id: this.memos[this.editIndex].id,
          title: memo.split('\n')[0],
          content: memo
        })
      }
      this.cancel()
    },
    cancel() {
      this.newMemo = undefined
      this.editIndex = null
    },
    deleteMemo() {
      if (this.editIndex !== null) {
        this.memos.splice(this.editIndex, 1)
        this.cancel()
      }
    },
    editMemo(index) {
      this.editIndex = index
      this.newMemo = this.memos[index].content
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

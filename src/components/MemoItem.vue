<template>
  <h1>All Memos</h1>
  <ul>
    <li v-for="(memo, index) in memos" :key="memo.id" @click="edit(index)">{{
        memo.content?.split('\n')[0] || null
      }}
    </li>
  </ul>
  <button @click="add">+</button>
  <div v-if="isDisplayingNewForm">
    <form @submit.prevent="create">
      <label>Add:</label>
      <textarea v-model="newMemo"></textarea>
      <button type="submit">Add</button>
    </form>
  </div>

  <div v-if="isDisplayingEditingForm">
    <form @submit.prevent="update">
      <label>Edit:</label>
      <textarea v-model="newMemo"></textarea>
      <button type="submit">Edit</button>
    </form>
    <button @click="destroy">Delete</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      memos: [],
      newMemo: undefined,
      selectedIndex: null
    }
  },
  mounted() {
    this.memos = JSON.parse(localStorage.getItem('memos')) || []
  },
  computed: {
    nextId() {
      return (this.memos[this.memos.length - 1]?.id || 0) + 1
    },
    isDisplayingNewForm() {
      return this.selectedIndex === -1
    },
    isDisplayingEditingForm() {
      return this.selectedIndex > 0
    }
  },
  methods: {
    add() {
      this.selectedIndex = -1
      this.newMemo =  '新規メモ'
    },
    create() {
      this.memos.push({
        id: this.nextId,
        content: this.newMemo
      })
      localStorage.setItem('memos', JSON.stringify(this.memos))
      this.$_cancel()
    },
    $_cancel() {
      this.newMemo = undefined
      this.selectedIndex = null
    },
    destroy() {
      if (this.selectedIndex !== null) {
        this.memos.splice(this.selectedIndex, 1)
        localStorage.setItem('memos', JSON.stringify(this.memos))
        this.$_cancel()
      }
    },
    edit(index) {
      this.selectedIndex = index
      this.newMemo = this.memos[index].content
    },
    update() {
      this.memos[this.selectedIndex] = {
        id: this.memos[this.selectedIndex].id,
        content: this.newMemo
      }
      localStorage.setItem('memos', JSON.stringify(this.memos))
      this.$_cancel()
    }
  }
}
</script>

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
      editIndex: null,
      updateIndex: null,
      isDisplayingNewForm: false
    }
  },
  mounted() {
    this.memos = JSON.parse(localStorage.getItem('memos')) || []
  },
  computed: {
    nextId() {
      return (this.memos[this.memos.length - 1]?.id || 0) + 1
    },
    isDisplayingEditingForm() {
      return Boolean(this.updateIndex)
    }
  },
  methods: {
    add() {
      this.isDisplayingNewForm = true
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
      this.isDisplayingNewForm = false
      this.updateIndex = null
    },
    destroy() {
      if (this.updateIndex !== null) {
        this.memos.splice(this.updateIndex, 1)
        localStorage.setItem('memos', JSON.stringify(this.memos))
        this.$_cancel()
      }
    },
    edit(index) {
      this.updateIndex = index
      this.newMemo = this.memos[index].content
    },
    update() {
      this.memos.splice(this.updateIndex, 1, {
        id: this.memos[this.updateIndex].id,
        content: this.newMemo
      })
      localStorage.setItem('memos', JSON.stringify(this.memos))
      this.$_cancel()
    }
  }
}
</script>

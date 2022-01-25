<template>
  <h1>All Memos</h1>
  <ul>
    <li v-for="(memo, index) in memos" :key="memo.id" @click="edit(index)">{{
        memo.content?.split('\n')[0] || null
      }}
    </li>
  </ul>
  <button @click="add">+</button>
  <div v-if="editIndex === null" v-show="isEditing">
    <form @submit.prevent="create">
      <label>Add:</label>
      <textarea v-model="newMemo"></textarea>
      <button type="submit">Add</button>
    </form>
  </div>

  <div v-else>
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
      nextId: undefined,
      isEditing: false
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
    this.nextId = this.memos[this.memos.length - 1]?.id || 0
  },
  methods: {
    add() {
      this.isEditing = true
      this.memos.push({
        id: ++this.nextId,
        content: '新規メモ'
      })
      this.newMemo = this.memos[this.memos.length - 1].content
    },
    create() {
      const index = this.memos.findIndex((memo) => memo.id === this.memos[this.memos.length - 1].id)
      this.memos.splice(index, 1, {
        id: this.memos[this.memos.length -1].id,
        content: this.newMemo
      })
      this.$_cancel()
    },
    $_cancel() {
      this.newMemo = undefined
      this.editIndex = null
      this.isEditing = false
    },
    destroy() {
      if (this.editIndex !== null) {
        this.memos.splice(this.editIndex, 1)
        this.$_cancel()
      }
    },
    edit(index) {
      this.editIndex = index
      this.newMemo = this.memos[index].content
    },
    update() {
      this.memos.splice(this.editIndex, 1, {
        id: this.memos[this.editIndex].id,
        content: this.newMemo
      })
      this.$_cancel()
    }
  }
}
</script>

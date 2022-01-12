<template>
  <h1>All Memos</h1>
  <ul>
    <li v-for="(memo, index) in memos" :key="memo.id" @click="editMemo(index)">{{
        memo.content?.split('\n')[0] || null
      }}
    </li>
  </ul>
  <button @click="addMemo">+</button>
  <div v-if="editIndex === null" v-show="isEditing">
    <form @submit.prevent="doneAddMemo">
      <label>Add:</label>
      <textarea v-model="newMemo"></textarea>
      <button type="submit">Add</button>
    </form>
  </div>

  <div v-else>
    <form @submit.prevent="doneEdit">
      <label>Edit:</label>
      <textarea v-model="newMemo"></textarea>
      <button type="submit">Edit</button>
    </form>
    <button @click="deleteMemo">Delete</button>
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
    addMemo() {
      this.isEditing = true
      this.memos.push({
        id: ++this.nextId,
        content: '新規メモ'
      })
      this.newMemo = this.memos[this.memos.length - 1].content
    },
    doneAddMemo() {
      const index = this.memos.findIndex((memo) => memo.id === this.memos[this.memos.length - 1].id)
      this.memos.splice(index, 1, {
        id: this.memos[this.memos.length -1].id,
        content: this.newMemo
      })
      this.cancel()
    },
    cancel() {
      this.newMemo = undefined
      this.editIndex = null
      this.isEditing = false
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
    },
    doneEdit() {
      this.memos.splice(this.editIndex, 1, {
        id: this.memos[this.editIndex].id,
        content: this.newMemo
      })
      this.cancel()
    }
  }
}
</script>

<style>

</style>

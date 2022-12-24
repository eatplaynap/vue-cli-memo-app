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
import { ref, computed, onMounted } from 'vue'

export default {
  setup() {
    const memos = ref([])
    const newMemo = ref(undefined)
    const selectedIndex = ref(undefined)

    const nextId = computed(() => (memos.value.length[memos.value.length - 1]?.id || 0) + 1)
    const isDisplayingNewForm = computed(()=> selectedIndex.value === -1 )
    const isDisplayingEditingForm = computed(()=> selectedIndex.value >= 0)

    onMounted(() => { memos.value = JSON.parse(localStorage.getItem('memos')) || [] })

    const add = () => {
      selectedIndex.value = -1
      newMemo.value = '新規メモ'
    }
    const create = () => {
      memos.value.push({
        id: nextId.value,
        content: newMemo.value
      })
      localStorage.setItem('memos', JSON.stringify(memos.value))
      $_cancel()
    }
    const $_cancel = () => {
      newMemo.value = undefined
      selectedIndex.value = undefined
    }
    const destroy = () => {
      if (selectedIndex.value !== undefined) {
        memos.value.splice(selectedIndex.value, 1)
        localStorage.setItem('memos', JSON.stringify(memos.value))
        $_cancel()
      }
    }
    const edit = (index) => {
      selectedIndex.value = index
      newMemo.value = memos.value[index].content
    }
    const update = () => {
      memos.value[selectedIndex.value] = {
        id: memos.value[selectedIndex.value].id,
        content: newMemo.value
      }
      localStorage.setItem('memos', JSON.stringify(memos.value))
      $_cancel()
    }

    return {
      memos,
      newMemo,
      selectedIndex,
      nextId,
      isDisplayingNewForm,
      isDisplayingEditingForm,
      add,
      create,
      destroy,
      edit,
      update
    }
  },
}
</script>

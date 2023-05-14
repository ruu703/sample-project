<script setup>
import { ref, computed } from 'vue'

const input = ref('')

const lists = ref(localStorage.getItem('lists') ? JSON.parse(localStorage.getItem('lists')) : [])

const addNewList = () => {
  if (!input.value) return
  lists.value.push({
    id: lists.value.length + 1,
    text: input.value,
    isChecked: false,
    delete: false,
    focus: false
  })
  setLocalStorage()
  input.value = ''
}

const todoList = computed(() => {
  return lists.value.filter((val) => !val.isChecked)
})

const deleteList = (id) => {
  lists.value = lists.value.filter((val) => {
    val.delete = false
    return id != val.id && !val.delete
  })
  setLocalStorage()
}

const ResetList = () => {
  lists.value = []
  input.value = ''
  localStorage.removeItem('lists')
}

const setLocalStorage = () => {
  localStorage.setItem('lists', JSON.stringify(lists.value))
}
</script>

<template>
  <div class="page">
    <div class="container">
      <h1 class="container__title">TODO LIST</h1>
      <div class="container__list text-left">
        <input
          type="text"
          id="katakana"
          v-model="input"
          class="w-2/3 shadow-lg"
          @keypress.enter="addNewList"
        />
        <button class="w-[calc((100%*1/6)-12px)] ml-3 h-14 p-1" @click="addNewList">submit</button>
        <button class="w-[calc((100%*1/6)-12px)] ml-3 h-14 p-1" @click="ResetList">Reset</button>
      </div>
      <ul class="divide-y divide-cyan-100 bg-sky-200 rounded shadow-lg relative w-full">
        <TransitionGroup>
          <li
            class="w-full text-left h-14 py-2 px-6 align-middle flex justify-between items-center"
            v-for="list in todoList"
            :key="list.id"
          >
            <input
              type="text"
              v-model="list.text"
              v-if="list.focus"
              class="bg-transparent border-0 pr-4 outline-none w-full"
              @keyup.enter.shift="list.focus = false"
            />
            <span
              :class="['overflow-auto px-4 w-full', { 'line-through': list.isChecked }]"
              @mouseover="list.focus = true"
              v-else-if="!list.focus"
              >{{ list.text }}</span
            >
            <font-awesome-icon
              :icon="['fas', 'trash-can']"
              @click="deleteList(list.id)"
              class="hover:scale-125 duration-150 cursor-pointer"
            />
          </li>
        </TransitionGroup>
      </ul>
    </div>
  </div>
</template>

<style scoped lang="scss">
button {
  @apply bg-amber-500 hover:bg-amber-400 hover:translate-x-1 hover:translate-y-1 hover:duration-100 duration-150 shadow-lg hover:shadow-md hover:shadow-amber-700/50 shadow-amber-600/50 text-white inline-block;
}
input[type='text'] {
  @apply h-14 inline-block;
}
.container {
  @apply py-14 w-1/2 mx-auto h-full;
  &__title {
    @apply text-sky-800 py-10;
  }
  &__list {
    @apply py-10;
  }
}
.page {
  @apply w-full
  h-screen
  bg-sky-300;
}
.v-enter-active {
  transition: opacity 0.5s ease;
}

.v-enter-from {
  opacity: 0;
}

.v-leave-active {
  transition: opacity 0.2s ease;
}

.v-leave-to {
  opacity: 0;
}
</style>

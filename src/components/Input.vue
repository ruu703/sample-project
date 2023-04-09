<script setup>
import { ref, computed } from 'vue'

const input = ref('')
const lists = ref([])
const addNewList = () => {
  if (!input.value) return
  lists.value.push({
    id: lists.value.length + 1,
    text: input.value,
    isChecked: false,
    delete: false
  })
  input.value = ''
}
const todoList = computed(() => {
  return lists.value.filter((val) => !val.isChecked)
})
// const deleteList = (id) => {
//   console.log(id)
//   lists.value = lists.value.filter((val) => {
//     val.delete = false
//     return id != val.id && !val.delete
//   })
// }
// const doneList = computed(() => {
//   return lists.value.filter((val) => val.isChecked)
// })
</script>

<template>
  <div class="page">
    <div class="container">
      <h1 class="container__title">TODO LIST</h1>
      <div class="container__list text-left">
        <input type="text" id="katakana" v-model="input" class="w-5/6 shadow-lg" />
        <button class="w-[calc((100%*1/6)-12px)] ml-3" @click="addNewList">submit</button>
      </div>
      <ul class="divide-y divide-cyan-100 bg-sky-200 rounded shadow-lg relative w-full">
        <TransitionGroup mode="out-in">
          <li
            class="w-full text-left h-14 py-2 px-6 align-middle flex justify-between items-center"
            v-for="list in todoList"
            :key="list.id"
          >
            <label>
              <input type="checkbox" class="hidden" v-model="list.isChecked" />
              <!-- <font-awesome-icon :icon="['far', 'circle']" size="lg" v-show="!list.isChecked" />
              <font-awesome-icon
                :icon="['fas', 'circle-check']"
                size="lg"
                v-show="list.isChecked"
              /> -->
            </label>

            <input
              type="text"
              v-model="list.text"
              v-if="list.delete"
              class="bg-transparent border-0 ml-2 pr-4 outline-none w-full"
              @mouseleave="list.delete = false"
            />
            <span
              :class="['overflow-auto px-4 ml-2 w-full', { 'line-through': list.isChecked }]"
              @mouseover="!list.isChecked ? (list.delete = true) : ''"
              v-else-if="!list.delete"
              >{{ list.text }}</span
            >

            <font-awesome-icon :icon="['fas', 'trash']" @click="deleteList(list.id)" />
          </li>
        </TransitionGroup>
      </ul>
      <!-- <h2 class="py-6">DONE</h2>
      <ul class="divide-y divide-cyan-100 bg-sky-200 rounded shadow-lg relative w-full">
        <TransitionGroup mode="out-in">
          <li
            class="w-full text-left h-14 py-2 px-6 align-middle flex justify-between items-center"
            v-for="list in doneList"
            :key="list.id"
          >
            <div class="w-full">
              <label>
                <input type="checkbox" class="hidden" v-model="list.isChecked" />
                <font-awesome-icon :icon="['far', 'circle']" size="lg" v-show="!list.isChecked" />
                <font-awesome-icon
                  :icon="['fas', 'circle-check']"
                  size="lg"
                  v-show="list.isChecked"
                />
              </label>
              <input
                type="text"
                v-model="list.text"
                v-if="list.delete"
                class="w-[60%] bg-transparent border-0 ml-2 outline-none"
                @mouseleave="list.delete = false"
              />
              <span
                :class="['overflow-auto w-full pl-4 ml-2', { 'line-through': list.isChecked }]"
                @mouseover="!list.isChecked ? (list.delete = true) : ''"
                v-else-if="!list.delete"
                >{{ list.text }}</span
              >
            </div>
            <font-awesome-icon :icon="['fas', 'trash']" @click="deleteList(list.id)" />
          </li>
        </TransitionGroup>
      </ul> -->
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
  transition: opacity 1s ease;
}

.v-enter-from {
  opacity: 0;
}

.v-leave-active {
  transition: opacity 1s ease;
}

.v-leave-to {
  opacity: 0;
}
</style>

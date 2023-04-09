<script setup lang="ts">
import TodoList from './views/TodoList.vue'
</script>

<template>
  <TodoList />
</template>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  & {
    :is(h1) {
      @apply text-5xl;
    }
    h2 {
      @apply text-4xl;
    }
    h3 {
      @apply text-3xl;
    }
    h4 {
      @apply text-2xl;
    }

    input {
      @apply py-2 px-4
      rounded;
      &[type='text'] {
        @apply border-spacing-0;
      }
    }

    button {
      @apply rounded
      p-4 h-14;
    }
  }
}
</style>

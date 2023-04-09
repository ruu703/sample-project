<script setup lang="ts">
import { onMounted } from 'vue'
onMounted(() => {
  // const element = document.querySelector('.top').children
  // const thirdElement = element[0].children[0]
  const element = document.getElementsByClassName('top')
  const second = element[0].children[0]
  const thirdElement = second.children[0]
  console.log(thirdElement)
  thirdElement.classList.add('red')
})
</script>

<template>
  <div class="top">
    aaa
    <div class="second1">
      <div class="third">中身</div>
    </div>
    <div class="second2">中身２</div>
  </div>
</template>

<style soped>
.red {
  color: red;
}
</style>

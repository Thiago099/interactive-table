<script setup lang="ts">
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
// import HelloWorld from './components/HelloWorld.vue'
import pagination from './components/pagination/pagination.vue'
import search from './components/search/search.vue'
import { ref } from 'vue'
const found = ref(true)
</script>

<template>

  <search :found="found" @input="event=>found = event.target.value.length < 5"></search>
  <pagination page="0" pages="30"></pagination>
  <!-- <HelloWorld msg="Hello Vue 3 + TypeScript + Vite" /> -->
</template>

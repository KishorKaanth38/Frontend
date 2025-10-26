<template>
  <div id="app">
    <h1>After-School Classes</h1>
    <LessonList />
  </div>
</template>

<script>
import LessonList from './components/icons/LessonList.vue'

export default {
  components: { LessonList }
}
</script>

<style>
body {
  font-family: Arial, sans-serif;
  margin: 20px;
  background-color: #f9f9f9;
}
h1 {
  text-align: center;
  margin-bottom: 20px;
}
</style>


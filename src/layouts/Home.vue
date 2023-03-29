<script setup>
import SearchBar from "../components/SearchBar.vue";
</script>
<template>
  <header>
    <img src="../assets/logo.png" alt="windbnb-logo" />
    <SearchBar />
  </header>
</template>
<style scoped>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
</style>

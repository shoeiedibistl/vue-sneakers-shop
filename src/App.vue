<script setup>
  import Header from "./components/Header.vue";
  import CardList from "./components/Cardlist.vue";
  import Drawer from "./components/Drawer.vue";
</script>

<template>
  <div
    class="w-[1080px] mx-auto my-10 bg-white rounded-4xl min-h-[calc(100vh-5rem)] shadow-xl"
  >
    <Drawer />
    <Header />
    <h1 class="text-xl text-4xl font-bold p-10">Все кроссовки</h1>
    <CardList />
  </div>
</template>

<style scoped></style>

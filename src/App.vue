<script setup>
  import MyHello from "./components/MyHello.vue";
</script>

<template>
  <MyHello title="кроссовки пума" :price="7000" is-added />
  <MyHello title="кроссовки абибас" :price="22000" :is-added="false" />
  <MyHello>
    <p>test slot</p>
  </MyHello>
  <MyHello>
    <MyHello />
  </MyHello>
</template>

<style scoped></style>

<script>
  export default {
    data() {
      return {
        count: 0,
      };
    },
    methods: {
      increment() {
        return this.count++;
      },
      decrement() {
        return this.count--;
      },
    },
  };
</script>

<template>
  <h1>{{ count }}</h1>
  <div class="flex gap-1">
    <button class="border-2 p-4" @click="decrement">-</button>
    <button class="border-2 p-4" @click="increment">+</button>
  </div>
  <template />
</template>

<style>
  .text {
    background-color: red;
  }
</style>

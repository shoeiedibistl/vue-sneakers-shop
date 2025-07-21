<script setup>
  defineProps({
    title: {
      type: String,
      default: "title",
    },
    price: Number,
    isAdded: {
      type: Boolean,
      default: null,
    },
  });
</script>

<template>
  <div class="product">
    <h2>Товар: {{ title }}</h2>
    <h3>Цена: {{ price }}</h3>
    <h3 v-if="isAdded">Добавлен: {{ isAdded }}</h3>

    <slot></slot>
  </div>
</template>

<style scoped>
  .product {
    padding: 16px;
    border: 1px solid blue;
    background-color: white;
  }

  .slot {
    border: 1px solid greenyellow;
    padding: 20px;
  }
</style>

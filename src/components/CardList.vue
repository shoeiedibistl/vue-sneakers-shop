<script setup>
  import Card from "./Card.vue";

  defineProps({
    items: Array,
  });

  const emit = defineEmits(["addToFavorite", "addToCart"]);
</script>

<template>
  <div class="grid grid-cols-4 gap-6 p-10">
    <Card
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :image-url="item.imageUrl"
      :title="item.title"
      :price="item.price"
      :on-click-add="() => emit('addToCart', item)"
      :is-favorite="item.isFavorite"
      :on-click-favorite="() => emit('addToFavorite', item)"
      :is-added="item.isAdded"
    />
  </div>
</template>

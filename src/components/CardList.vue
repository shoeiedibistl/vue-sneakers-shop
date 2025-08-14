<script setup>
  import Card from "./Card.vue";

  defineProps({
    items: Array,
    isFavorites: Boolean,
  });

  const emit = defineEmits(["addToFavorite", "addToCart"]);
</script>

<template>
  <div class="grid grid-cols-4 gap-6 p-10" v-auto-animate>
    <Card
      v-for="item in items"
      :key="item.id + Math.random()"
      :id="item.id"
      :image-url="item.imageUrl"
      :title="item.title"
      :price="item.price"
      :on-click-add="isFavorites ? null : () => emit('addToCart', item)"
      :is-favorite="item.isFavorite"
      :on-click-favorite="
        isFavorites ? null : () => emit('addToFavorite', item)
      "
      :is-added="item.isAdded"
    />
  </div>
</template>

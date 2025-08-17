<script setup>
  defineProps({
    title: String,
    description: String,
    imageUrl: String,
    imageSize: {
      type: Number,
      default: 120,
    },
  });
</script>

<template>
  <div class="my-auto">
    <img
      :src="imageUrl"
      alt="info image"
      :height="imageSize"
      :width="imageSize"
      class="mx-auto"
    />
    <h2 class="mt-4 text-2xl font-medium text-center">{{ title }}</h2>
    <p class="text-center mt-2 text-gray-400">{{ description }}</p>
  </div>
</template>

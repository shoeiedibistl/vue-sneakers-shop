<script setup>
  import Card from "./Card.vue";
  import { inject } from "vue";

  const addToFavorite = inject("addToFavorite");

  defineProps({
    items: Array,
  });

  const onClickAdd = () => {
    alert("add");
  };

  const onCLickFavorite = () => {
    const obj = {
      ...props,
      parentId: props.id,
    };

    addToFavorite(obj);
  };
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
      :on-click-add="onClickAdd"
      :is-favorite="item.isFavorite"
      :on-click-favorite="() => addToFavorite(item)"
    />
  </div>
</template>

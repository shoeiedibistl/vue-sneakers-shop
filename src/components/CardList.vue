<script setup>
  import Card from "./Card.vue";

  const onClickAdd = () => console.log(this);
</script>

<template>
  <div class="grid grid-cols-4 gap-6 p-10">
    <Card
      image-url="/sneakers/sneakers-1.jpg"
      title="Мужские Кроссовки Nike Blazer Mid Suede"
      :price="5000"
      :is-added="true"
      :is-favorite="true"
      :on-click-add="onClickAdd"
    />
  </div>
</template>

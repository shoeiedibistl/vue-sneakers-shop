<script setup>
  import { onMounted, ref } from "vue";
  import axios from "axios";
  import CardList from "@/components/CardList.vue";

  const favorites = ref([]);

  //допилить запрос закладок из общего списка через связку ресурсов на моккидев
  //допилить CardList пропсы @add-to-favorite="addToFavorite", @add-to-cart="onClickAddPlus"
  //сейчас не рендерятся кнопки в карточке

  onMounted(async () => {
    try {
      const { data } = await axios.get(
        `https://c54d42806c01eb8f.mokky.dev/favorites?_relations=items`
      );

      favorites.value = data.map((obj) => obj.item);

      console.log(data);
    } catch (err) {
      console.log(err);
    }
  });
</script>

<template>
  <h1 class="text-xl text-4xl font-bold">Мои закладки</h1>

  <CardList :items="favorites" isFavorites />
</template>

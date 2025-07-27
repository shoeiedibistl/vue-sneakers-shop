<script setup>
  // формат запроса по нескольким словам в поиске https://c54d42806c01eb8f.mokky.dev/items?title=*Кроссовки*%20*Puma - между словами требуется символ пробела (%20)
  import Header from "./components/Header.vue";
  import CardList from "./components/Cardlist.vue";
  import Drawer from "./components/Drawer.vue";

  import { onMounted, reactive, ref, watch } from "vue";
  import axios from "axios";

  const items = ref([]);

  const filters = reactive({
    sortBy: "",
    searchQuery: "",
  });

  const onChangeSelect = (event) => {
    filters.sortBy = event.target.value;
  };

  const onChangeSearchInput = (event) => {
    filters.searchQuery = event.target.value;
  };

  const fetchItems = async () => {
    const params = {
      sortBy: filters.sortBy,
    };

    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}`;
    }

    try {
      const { data } = await axios.get(
        `https://c54d42806c01eb8f.mokky.dev/items?`,
        {
          params,
        }
      );
      items.value = data;
    } catch (err) {
      console.log(err);
    }
  };

  onMounted(fetchItems);

  watch(filters, fetchItems);
</script>

<template>
  <div
    class="w-[1080px] mx-auto my-10 bg-white rounded-4xl min-h-[calc(100vh-5rem)] shadow-xl"
  >
    <!--
    <Drawer />
    -->
    <Header />
    https://youtu.be/U_-Ht_v-oAs?si=K26i4wLVlfDrHlbo&t=13806

    <div class="flex gap-10 items-center justify-between px-10 mt-10">
      <h1 class="text-xl text-4xl font-bold">Все кроссовки</h1>

      <div class="flex items-center gap-5">
        <select
          v-on:change="onChangeSelect"
          name=""
          id=""
          class="px-5 py-2 border border-slate-300 rounded-xl outline-none cursor-pointer"
        >
          <option value="title">По названию</option>
          <option value="price">По цене (дешевле)</option>
          <option value="-price">По цене (дороже)</option>
        </select>

        <div class="flex relative">
          <img
            src="/search.svg"
            alt=""
            class="absolute top-1/2 left-3 -translate-y-1/2 cursor-pointer"
          />

          <input
            @input="onChangeSearchInput"
            type="text"
            class="border border-slate-300 rounded-xl outline-none py-2 pl-10 pr-4 duration-300 focus:border-gray-400"
            placeholder="Поиск..."
          />
        </div>
      </div>
    </div>
    <CardList :items="items" />
  </div>
</template>

<style scoped></style>

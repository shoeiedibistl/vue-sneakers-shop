<script setup>
  import {
    onMounted,
    provide,
    reactive,
    inject,
    ref,
    watch,
    computed,
  } from "vue";
  import axios from "axios";
  import debounce from "lodash.debounce";
  import CardList from "@/components/CardList.vue";

  const { cart, addToCart, removeFromCart } = inject("cart");

  const items = ref([]);

  const filters = reactive({
    sortBy: "",
    searchQuery: "",
  });

  const onClickAddPlus = (item) => {
    if (item.isAdded) {
      removeFromCart(item);
    } else {
      addToCart(item);
    }
  };

  const onChangeSelect = (event) => {
    filters.sortBy = event.target.value;
  };

  const onChangeSearchInput = debounce((event) => {
    filters.searchQuery = event.target.value;
  }, 500);

  const addToFavorite = async (item) => {
    try {
      if (!item.isFavorite) {
        const obj = {
          item_id: item.id,
        };

        const { data } = await axios.post(
          `https://c54d42806c01eb8f.mokky.dev/favorites`,
          obj
        );

        item.isFavorite = true;
        item.favoriteId = data.id;

        //   console.log(item);
      } else {
        //   console.log(item);

        await axios.delete(
          `https://c54d42806c01eb8f.mokky.dev/favorites/${item.favoriteId}`
        );
        item.isFavorite = false;
        item.favoriteId = null;
      }
    } catch (err) {
      console.log(err);
    }
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
      items.value = data.map((obj) => ({
        ...obj,
        isFavorite: false,
        isAdded: false,
        favoriteId: null,
      }));
    } catch (err) {
      console.log(err);
    }
  };

  const fetchFavorites = async () => {
    try {
      const { data: favorites } = await axios.get(
        `https://c54d42806c01eb8f.mokky.dev/favorites`
      );

      items.value = items.value.map((item) => {
        const favorite = favorites.find(
          (favorite) => favorite.item_id === item.id
        );

        if (!favorite) {
          return item;
        }

        return {
          ...item,
          isFavorite: true,
          favoriteId: favorite.id,
        };
      });
    } catch (err) {
      console.log(err);
    }
  };

  onMounted(async () => {
    const localCart = localStorage.getItem("cart");
    cart.value = localCart ? JSON.parse(localCart) : [];

    await fetchItems();
    await fetchFavorites();

    items.value = items.value.map((item) => ({
      ...item,
      isAdded: cart.value.some((cartItem) => cartItem.id === item.id),
    }));
  });

  watch(cart, () => {
    items.value = items.value.map((item) => ({
      ...item,
      isAdded: false,
    }));
  });

  watch(filters, fetchItems);
</script>

<template>
  <div class="flex gap-10 items-center justify-between">
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
  <CardList
    :items="items"
    @add-to-favorite="addToFavorite"
    @add-to-cart="onClickAddPlus"
  />
</template>

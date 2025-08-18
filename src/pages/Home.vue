<script setup>
  import { onMounted, reactive, inject, ref, watch, computed } from "vue";
  import axios from "axios";
  import debounce from "lodash.debounce";
  import CardList from "@/components/CardList.vue";

  const { cart, addToCart, removeFromCart } = inject("cart");

  const items = ref([]);
  const myFavorites = ref([]);

  const itemsWithCartStatus = computed(() => {
    return items.value.map((item) => ({
      ...item,
      isAdded: cart.value.some((cartItem) => cartItem.id === item.id),
    }));
  });

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

        myFavorites.value.push({
          id: item.favoriteId,
          item_id: item.id,
        });
      } else {
        await axios.delete(
          `https://c54d42806c01eb8f.mokky.dev/favorites/${item.favoriteId}`
        );
        item.isFavorite = false;
        item.favoriteId = null;

        myFavorites.value.splice(
          myFavorites.value.findIndex(
            (myFavoriteItem) => myFavoriteItem.item_id === item.id
          ),
          1
        );
      }
    } catch (err) {
      console.log(err);
    }
    //finally {
    //  fetchFavorites();
    //  console.log("myFavorites.value", myFavorites.value);

    //  const myCard = myFavorites.value.find(
    //    (favorite) => favorite.item_id === item.id
    //  );

    //  console.log("63 myCard", myCard);

    //   myCard.isFavorite = item.isFavorite;
    //   myCard.favoriteId = item.favoriteId;

    //  console.log("items", items.value);
    //  console.log("myFavorites", myFavorites.value);
    // }
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
        isFavorite:
          myFavorites.value.length < 1
            ? false
            : myFavorites.value.some((fav) => fav.item_id === obj.id),
        // isAdded: false,
        isAdded: cart.value.some((item) => item.id === obj.id),

        favoriteId:
          myFavorites.value.length < 1
            ? null
            : myFavorites.value.filter((fav) => fav.item_id === obj.id)[0]
                ?.id || null,
      }));
    } catch (err) {
      console.log(err);
    }
  };

  const fetchFavorites = async () => {
    try {
      const { data: myData } = await axios.get(
        `https://c54d42806c01eb8f.mokky.dev/favorites`
      );

      myFavorites.value = myData;

      items.value = items.value.map((item) => {
        const favorite = myFavorites.value.find(
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

    // items.value = items.value.map((item) => ({
    //   ...item,
    //   isAdded: cart.value.some((cartItem) => cartItem.id === item.id),
    // }));
  });

  watch(cart, () => {
    items.value = items.value.map((item) => ({
      ...item,
      isAdded: false,
      //  isAdded: cart.value.some((item) => item.id === obj.id),
    }));
  });

  //  watch(cart, fetchItems, { deep: true });

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
    :items="itemsWithCartStatus"
    @add-to-favorite="addToFavorite"
    @add-to-cart="onClickAddPlus"
  />
</template>

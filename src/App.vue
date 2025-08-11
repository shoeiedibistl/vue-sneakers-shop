<script setup>
  // формат запроса по нескольким словам в поиске https://c54d42806c01eb8f.mokky.dev/items?title=*Кроссовки*%20*Puma - между словами требуется символ пробела (%20)
  import Header from "./components/Header.vue";
  import CardList from "./components/CardList.vue";
  import Drawer from "./components/Drawer.vue";

  import { onMounted, provide, reactive, ref, watch, computed } from "vue";
  import axios from "axios";

  const items = ref([]);
  const cart = ref([]);

  const drawerOpenFlag = ref(false);
  const isCreatingOrder = ref(false);

  const totalPrice = computed(() =>
    cart.value.reduce((summ, item) => summ + item.price, 0)
  );

  const vatPrice = computed(() => Math.ceil(totalPrice.value * 0.05));

  const closeDrawer = () => {
    drawerOpenFlag.value = false;
  };

  const openDrawer = () => {
    drawerOpenFlag.value = true;
  };

  const addToCart = (item) => {
    if (!item.isAdded) {
      cart.value.push(item);
      item.isAdded = true;
    }
  };

  const removeFromCart = (item) => {
    cart.value.splice(cart.value.indexOf(item), 1);
    item.isAdded = false;
  };

  const onClickAddPlus = (item) => {
    if (item.isAdded) {
      removeFromCart(item);
    } else {
      addToCart(item);
    }
  };

  // provide("closeDrawer", closeDrawer);
  // provide("openDrawer", openDrawer);

  provide("cart", {
    cart,
    closeDrawer,
    openDrawer,
    addToCart,
    removeFromCart,
    //   drawerOpenFlag,
  });
  //provide("drawerOpenFlag", drawerOpenFlag);

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
          (favorite) => favorite.parentId === item.id
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

  const addToFavorite = async (item) => {
    try {
      if (!item.isFavorite) {
        const obj = {
          parentId: item.id,
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

  const createOrder = async () => {
    try {
      isCreatingOrder.value = true;
      const { data } = await axios.post(
        `https://c54d42806c01eb8f.mokky.dev/orders`,
        {
          items: cart.value,
          totalPrice: totalPrice.value,
        }
      );

      cart.value = [];

      return data;
    } catch (err) {
      console.log(err);
    } finally {
      isCreatingOrder.value = false;
    }
  };

  const cartButtonDisabled = computed(
    () => isCreatingOrder.value || cart.value.length === 0
  );

  onMounted(async () => {
    await fetchItems();
    await fetchFavorites();
  });

  watch(filters, fetchItems);

  watch(drawerOpenFlag, () => {
    if (drawerOpenFlag.value) {
      document.body.style.overflow = "hidden";
    } else {
      document.body.style.overflow = "";
    }
  });

  watch(cart, () => {
    items.value = items.value.map((item) => ({
      ...item,
      isAdded: false,
    }));
  });
</script>

<template>
  <div
    class="w-[1080px] mx-auto my-10 bg-white rounded-4xl min-h-[calc(100vh-5rem)] shadow-xl"
  >
    <Drawer
      v-show="drawerOpenFlag"
      :total-price="totalPrice"
      :vat-price="vatPrice"
      @create-order="createOrder"
      :button-disabled="cartButtonDisabled"
    />

    <Header @open-drawer="openDrawer" :total-price="totalPrice" />
    <a
      href="https://youtu.be/U_-Ht_v-oAs?si=jMaE-r0h2QsBDytu&t=21221"
      class="hover:text-blue-700 duration-300"
      target="_blank"
      >https://youtu.be/U_-Ht_v-oAs?si=jMaE-r0h2QsBDytu&t=21221</a
    >

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
    <CardList
      :items="items"
      @add-to-favorite="addToFavorite"
      @add-to-cart="onClickAddPlus"
    />
  </div>
</template>

<style scoped></style>

<script setup>
  // формат запроса по нескольким словам в поиске https://c54d42806c01eb8f.mokky.dev/items?title=*Кроссовки*%20*Puma - между словами требуется символ пробела (%20)
  import Header from "./components/Header.vue";
  import Drawer from "./components/Drawer.vue";
  import Home from "./pages/Home.vue";

  import { onMounted, provide, ref, watch, computed } from "vue";
  import axios from "axios";

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

  watch(drawerOpenFlag, () => {
    if (drawerOpenFlag.value) {
      document.body.style.overflow = "hidden";
    } else {
      document.body.style.overflow = "";
    }
  });

  watch(
    cart,
    () => {
      localStorage.setItem("cart", JSON.stringify(cart.value));
    },
    { deep: true }
  );
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
      href="https://youtu.be/U_-Ht_v-oAs?si=NGziC4rdHbS_whh8&t=25961"
      class="hover:text-blue-700 duration-300"
      target="_blank"
      >https://youtu.be/U_-Ht_v-oAs?si=NGziC4rdHbS_whh8&t=25961</a
    >

    <div class="p-10"><RouterView /></div>
  </div>
</template>

<style scoped></style>

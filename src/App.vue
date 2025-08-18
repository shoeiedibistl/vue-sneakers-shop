<script setup>
  // формат запроса по нескольким словам в поиске https://c54d42806c01eb8f.mokky.dev/items?title=*Кроссовки*%20*Puma - между словами требуется символ пробела (%20)
  import Header from "./components/Header.vue";
  import Drawer from "./components/Drawer.vue";

  import { provide, ref, watch, computed } from "vue";

  const cart = ref([]);

  const drawerOpenFlag = ref(false);

  const orderId = ref(null);

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
    if (
      !item.isAdded
      // &&
      //!cart.value.some((cartItem) => cartItem.id === item.id)
    ) {
      cart.value.push(item);
      item.isAdded = true;
    }

    if (orderId.value) {
      orderId.value = null;
    }

    console.log("cart", cart.value);
  };

  const removeFromCart = (item) => {
    // const itemToRemove = cart.value.find((cartItem) => cartItem.id === item.id);

    //  cart.value.splice(cart.value.indexOf(item), 1);
    // cart.value.splice(cart.value.indexOf(itemToRemove), 1);

    // item.isAdded = false;

    cart.value = cart.value.filter((cartItem) => cartItem.id !== item.id);

    console.log(cart.value);
  };

  // provide("closeDrawer", closeDrawer);
  // provide("openDrawer", openDrawer);

  provide("cart", {
    cart,
    closeDrawer,
    openDrawer,
    addToCart,
    removeFromCart,
    orderId,
    //   drawerOpenFlag,
  });
  //provide("drawerOpenFlag", drawerOpenFlag);

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
    />

    <Header @open-drawer="openDrawer" :total-price="totalPrice" />

    <div class="p-10"><RouterView /></div>
  </div>
</template>

<style scoped></style>

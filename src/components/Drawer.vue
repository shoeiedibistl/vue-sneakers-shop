<script setup>
  import { inject } from "vue";

  const closeDrawer = inject("closeDrawer");

  import DrawerHead from "./DrawerHead.vue";
  import CartItemList from "./CartItemList.vue";
</script>

<template>
  <div class="drawer-wrapper">
    <div
      class="fixed inset-0 bg-black opacity-60 z-10"
      @click="closeDrawer"
    ></div>

    <div
      class="bg-white w-96 right-0 top-0 bottom-0 fixed z-10 p-8 flex flex-col gap-6"
    >
      <DrawerHead />

      <CartItemList />

      <div class="flex flex-col gap-3 mt-auto">
        <div class="flex gap-1">
          <span>Итого:</span>
          <div class="flex-1 border-b border-dotted border-gray-700"></div>
          <b>12000 руб.</b>
        </div>

        <div class="flex gap-1">
          <span>Налог 5%:</span>
          <div class="flex-1 border-b border-dotted border-gray-700"></div>
          <b>900 руб.</b>
        </div>

        <button
          disabled
          class="bg-lime-500 p-3 w-full text-white rounded-xl duration-300 hover:bg-lime-600 active:bg-lime-700 disabled:bg-slate-300 cursor-pointer mt-5"
        >
          Оформить заказ
        </button>
      </div>
    </div>
  </div>
</template>

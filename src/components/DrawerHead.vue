<script setup>
  import { inject } from "vue";

  const closeDrawer = inject("closeDrawer");
</script>

<template>
  <div class="flex items-center gap-5">
    <svg
      class="opacity-40 cursor-pointer rotate-180 duration-300 hover:opacity-100 hover:-translate-x-1"
      width="16"
      height="14"
      viewBox="0 0 16 14"
      fill="none"
      xmlns="http://www.w3.org/2000/svg"
      @click="closeDrawer"
    >
      <path
        d="M1 7H14.7143"
        stroke="black"
        stroke-width="2"
        stroke-linecap="round"
        stroke-linejoin="round"
      />
      <path
        d="M8.71436 1L14.7144 7L8.71436 13"
        stroke="black"
        stroke-width="2"
        stroke-linecap="round"
        stroke-linejoin="round"
      />
    </svg>

    <h2 class="text-2xl font-bold">Корзина</h2>
  </div>
</template>

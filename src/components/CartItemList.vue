<script setup>
  import { inject } from "vue";
  import CartItem from "./CartItem.vue";

  const { cart, removeFromCart } = inject("cart");
</script>

<template>
  <div class="flex flex-col gap-5 flex-1 overflow-y-auto" v-auto-animate>
    <CartItem
      v-for="item in cart"
      :image-url="item.imageUrl"
      :title="item.title"
      :price="item.price"
      :key="item.id + 'cart-item'"
      @on-click-remove="() => removeFromCart(item)"
      :size="item.size"
    />
  </div>
</template>

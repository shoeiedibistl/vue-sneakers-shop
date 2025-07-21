<script setup></script>

<template>
  <div
    class="w-4/5 mx-auto my-10 bg-white rounded-4xl min-h-[calc(100vh-5rem)] shadow-xl"
  >
    <header
      class="flex justify-between items-center border-b border-slate-300 p-10"
    >
      <div class="flex items-center gap-4">
        <img src="/logo.png" alt="Logo" class="w-10 h-10 shrink-0" />
        <div>
          <h2 class="text-xl font-bold uppercase">Vue sneakers</h2>
          <p class="text-slate-500">Магазин лучших кроссовок</p>
        </div>
      </div>

      <ul class="flex items-center gap-10">
        <li class="flex items-center gap-2 cursor-pointer">
          <img src="/cart.svg" alt="cart" />
          <b>1000 руб</b>
        </li>
        <li class="flex items-center gap-2 cursor-pointer">
          <img src="/heart.svg" alt="user" />
          <b>Закладки</b>
        </li>
        <li class="flex items-center gap-2 cursor-pointer">
          <img src="/profile.svg" alt="user" />
          <b>Профиль</b>
        </li>
      </ul>
    </header>
  </div>
</template>

<style scoped></style>

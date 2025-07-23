<template>
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
      <li
        class="flex items-center gap-2 cursor-pointer text-gray-400 hover:text-black transition:color duration-300"
      >
        <img src="/cart.svg" alt="cart" />
        <b>1000 руб</b>
      </li>
      <li
        class="flex items-center gap-2 cursor-pointer text-gray-400 hover:text-black transition:color duration-300"
      >
        <img src="/heart.svg" alt="user" />
        <b>Закладки</b>
      </li>
      <li
        class="flex items-center gap-2 cursor-pointer text-gray-400 hover:text-black transition:color duration-300"
      >
        <img src="/profile.svg" alt="user" />
        <b>Профиль</b>
      </li>
    </ul>
  </header>
</template>

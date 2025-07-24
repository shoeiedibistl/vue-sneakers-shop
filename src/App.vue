<script setup>
  import Header from "./components/Header.vue";
  import CardList from "./components/Cardlist.vue";
  import Drawer from "./components/Drawer.vue";
</script>

<template>
  <div
    class="w-[1080px] mx-auto my-10 bg-white rounded-4xl min-h-[calc(100vh-5rem)] shadow-xl"
  >
    <!--
    <Drawer />
    -->
    <Header />

    <div class="flex gap-10 items-center justify-between px-10 mt-10">
      <h1 class="text-xl text-4xl font-bold">Все кроссовки</h1>

      <div class="flex items-center gap-5">
        <select
          name=""
          id=""
          class="px-5 py-2 border border-slate-300 rounded-xl outline-none cursor-pointer"
        >
          <option value="">По названию</option>
          <option value="">По цене (дешевле)</option>
          <option value="">По цене (дороже)</option>
        </select>

        <div class="flex relative">
          <img
            src="/search.svg"
            alt=""
            class="absolute top-1/2 left-3 -translate-y-1/2 cursor-pointer"
          />

          <input
            type="text"
            class="border border-slate-300 rounded-xl outline-none py-2 pl-10 pr-4 duration-300 focus:border-gray-400"
            placeholder="Поиск..."
          />
        </div>
      </div>
    </div>
    <CardList />
  </div>
</template>

<style scoped></style>

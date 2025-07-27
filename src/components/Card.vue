<script setup>
  defineProps({
    imageUrl: {
      type: String,
      default: "/image-placeholder.jpg",
    },
    title: {
      type: String,
      default: "sneakers title",
    },
    price: Number,
    isAdded: Boolean,
    isFavorite: Boolean,
    onClickAdd: Function,
    onCLickFavorite: Function,
  });

  const getAlert = () => alert("click");
</script>

<template>
  <div
    class="flex flex-col border border-gray-200 rounded-3xl py-5 px-8 items-center gap-[14px] relative hover:-translate-y-3 hover:shadow-2xl duration-300 bg-white h-full"
  >
    <img
      :src="!isFavorite ? '/like-1.svg' : '/like-2.svg'"
      alt="like"
      class="absolute top-5 left-5 cursor-pointer"
      @click="onCLickFavorite"
    />
    <img :src="imageUrl" alt="sneakers" class="w-[133px] h-113px" />
    <p class="text-[14px] font-regular">{{ title }}</p>
    <div class="flex justify-between items-end w-full mt-auto">
      <div class="flex flex-col gap-0.5">
        <p class="text-gray-500 text-[11px]">Цена</p>
        <p class="text-[14px] font-bold">{{ price }} руб.</p>
      </div>
      <img
        :src="!isAdded ? '/plus.svg' : '/checked.svg'"
        alt="plus"
        class="cursor-pointer"
        @click="onClickAdd"
      />
    </div>
  </div>
</template>

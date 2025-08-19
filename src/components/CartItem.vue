<script setup>
  defineProps({
    id: Number,
    imageUrl: String,
    title: String,
    price: Number,
    size: {
      type: Number,
      default: 0,
    },
  });

  const emit = defineEmits(["onClickRemove"]);
</script>

<template>
  <div class="w-full p-4 border border-slate-300 rounded-xl flex gap-5">
    <img :src="imageUrl" :alt="title" class="w-[70px] h-[70px]" />
    <div class="flex flex-col gap-4 grow">
      <p>{{ title }}</p>
      <p>{{ size }} размер</p>
      <div class="flex justify-between items-end">
        <b class="flex-1">{{ price }} руб.</b>

        <img
          src="/close.svg"
          alt=""
          class="cursor-pointer"
          @click="emit('onClickRemove')"
        />
      </div>
    </div>
  </div>
</template>

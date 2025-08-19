<script setup>
  import { onMounted, ref, watch } from "vue";

  const props = defineProps({
    id: Number,
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
    onClickFavorite: Function,
    sizes: {
      type: Array,
      default: () => [],
      required: false,
    },
  });

  const emit = defineEmits(["size-changed"]);

  const selectedSize = ref(null);

  let initialSize = props.sizes.find((size) => size.inStock);

  const notifySizeChange = () => {
    emit("size-changed", {
      productId: props.id,
      size: selectedSize.value,
    });
  };

  const selectSize = (e) => {
    selectedSize.value = e.target.value;

    notifySizeChange();
  };

  watch(selectedSize, (newSize) => {
    console.log("new size", newSize);
  });

  onMounted(() => {
    initialSize = props.sizes.find((size) => size.inStock);

    if (initialSize) {
      selectedSize.value = initialSize.size;

      notifySizeChange();
    }
  });
</script>

<template>
  <div
    :key="`card-${id}`"
    class="flex flex-col border border-gray-200 rounded-3xl py-5 px-8 items-center gap-[14px] relative hover:shadow-2xl duration-300 bg-white h-full will-change-transform"
  >
    <img
      v-show="onClickFavorite"
      :src="!isFavorite ? '/like-1.svg' : '/like-2.svg'"
      alt="like"
      class="absolute top-5 left-5 cursor-pointer hover:drop-shadow duration-300"
      @click="onClickFavorite"
    />
    <img :src="imageUrl" alt="sneakers" class="w-[133px] h-113px" />
    <p class="text-[14px] font-regular">{{ title }}</p>

    <!---->
    <select
      v-if="initialSize"
      class="w-full"
      name="size"
      :id="'select-' + id"
      v-model="selectedSize"
    >
      <option
        v-for="sizeItem in sizes"
        :value="sizeItem.size"
        :disabled="!sizeItem.inStock"
        :class="sizeItem.inStock ? '' : 'opacity-20 text-gray-400'"
      >
        {{ sizeItem.size }}
      </option>
    </select>

    <div v-else>Нет в наличии</div>

    <div v-if="initialSize">выбран размер {{ selectedSize }}</div>

    <div class="flex justify-between items-end w-full mt-auto">
      <div class="flex flex-col gap-0.5">
        <p class="text-gray-500 text-[11px]">Цена</p>

        <p class="text-[14px] font-bold">{{ price }} руб.</p>
      </div>
      <img
        v-show="onClickAdd"
        :src="!isAdded ? '/plus.svg' : '/checked.svg'"
        alt="plus"
        class="cursor-pointer hover:drop-shadow duration-300"
        @click="onClickAdd"
      />
    </div>
  </div>
</template>

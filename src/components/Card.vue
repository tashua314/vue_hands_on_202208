<template>
    <div class="thumbnail">
        <img
        :src="image"
        alt="">
    </div>
    <div class="description">
        <h2>{{ name }}</h2>
        <p>{{ description }}</p>
        <span>¥<span class="price">{{ pricePrefix(price) }}</span></span>
    </div>
    <template v-if="no_item">
        <button type="button" @click="() => emit('stock-item', id)">在庫補充</button>
    </template>
    <template v-else>
        <button type="button" @click="() => emit('sold-out', id)">売り切れ</button>
    </template>
</template>

<script setup lang="ts">
defineProps({
  id: {
    type: Number,
    default: null,
    required: false
  },
  name: {
    type: String,
    default: '',
    required: false
  },
  description: {
    type: String,
    default: '',
    required: false
  },
  price: {
    type: Number,
    default: 0,
    required: false
  },
  image: {
    type: String,
    default: '',
    required: false
  },
  no_item: {
    type: Boolean,
    default: false,
    required: false
  }
});

/**
 * 価格を3桁ごとのカンマ付きで返す
 * @param {number} price 価格
 */
function pricePrefix(price: number) {
  return price.toLocaleString()
}

const emit = defineEmits(['sold-out', 'stock-item'])
</script>

<style>
.item > div.thumbnail > img {
  width: 100%;
  height: calc(100%);
  object-fit: cover;
}
.item > div.description {
  text-align: left;
  margin-top: 20px;
}
.item > div.description > p {
  margin-top: 0px;
  margin-bottom: 0px;
  font-size: 18px;
  line-height: 25px;
}
.item > div.description > span {
  display: block;
  margin-top: 10px;
  font-size: 20px;
}
.item > div.description > span > .price {
  font-size: 28px;
  font-weight: bold;
}
</style>
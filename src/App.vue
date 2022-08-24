<script setup lang="ts">
import { reactive, computed } from 'vue';
import Card from './components/Card.vue';
interface Item {
  id: number;
  name: string;
  description: string;
  price: number;
  image: string;
  soldOut: boolean;
  selected: boolean;
}
const items = reactive([
  {
    id: 1,
    name: 'アボカドディップバケット',
    description:
      '刻んだ野菜をアボカドと混ぜてディップに。こんがり焼いたバゲットとお召し上がりください。',
    price: 480,
    image: '/images/item1.jpg',
    soldOut: false,
    selected: false
  },
  {
    id: 2,
    name: 'あの日夢見たホットケーキ',
    description:
      '子供のころに食べたかった、あのホットケーキを再現しました。素朴でどこか懐かしい味をどうぞ。',
    price: 1180,
    image: '/images/item2.jpg',
    soldOut: false,
    selected: false
  },
  {
    id: 3,
    name: 'HOP WTR',
    description:
      'ロサンゼルス生まれのスパークリングウォーター。ノンカロリー、ノンアルコールの新感覚飲料です。',
    price: 320,
    image: '/images/item3.jpg',
    soldOut: true
  },
  {
    id: 4,
    name: 'チーズフレンチフライ',
    description:
      'イタリア産チーズをたっぷりかけたアツアツのフレンチフライ。みんな大好きな一品です。',
    price: 670,
    image: '/images/item4.jpg',
    soldOut: false,
    selected: false
  }
] as Item[])

/**
 * 在庫のある商品数を返す
 */
const stockQuantityComputed = computed(function() {
  return items.filter(item => item.soldOut === false ).length
})

/**
 * 商品の在庫状況を変更する
 * @param {object} 商品情報
 */
function stockItem(id: number) {
  const pickElm = items.find(item => item.id == id)
  if (pickElm) pickElm.soldOut = false
}

function changeSoldOut(id: number) {
  const pickElm = items.find(item => item.id == id)
  if (pickElm) pickElm.soldOut = true
}
</script>

<template>
  <header class="header">
    <img
      src="/images/logo.svg"
      alt="">
    <h1>Vue.js ハンズオン</h1>
  </header>
  <div>商品数：{{ stockQuantityComputed }}</div>
  <main class="main">
    <template
      v-for="item in items"
      :key="item.id">
      <div
        class="item"
        :class="{
          'selected-item': item.selected,
          'sold-out': item.soldOut
        }"
        @keyup.enter="item.selected = !item.selected"
        @click="item.selected = !item.selected"
        tabindex="0"
        >
        <Card
          :id="item.id"
          :image="item.image"
          :name="item.name"
          :description="item.description"
          :price="item.price"
          :no_item="item.soldOut"
           @sold-out="changeSoldOut"
           @stock-item="stockItem"/>
      </div>
    </template>
  </main>
</template>

<style>
body {
  font-family: sans-serif;
  margin: 0;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

#app {
  width: 90%;
  margin: 0 5%;
  color: #242424;
}

.header {
  display: flex;
  align-content: center;
  align-items: center;
  margin-top: 40px;
  margin-bottom: 40px;
}

.header > img {
  width: 100px;
  height: 100px;
  margin-right: 20px;
}

.header > h1 {
  font-size: 80px;
  font-weight: bold;
  line-height: 80px;
  margin-top: 0;
  margin-bottom: 0;
}

.main {
  display: grid;
  grid-template-columns: 3fr 3fr 3fr 3fr;
  column-gap: 24px;
  row-gap: 24px;
}

.item {
  padding: 10px;
  cursor: pointer;
}

.item:hover {
  transition: 0.2s transform ease-out;
  transform: scale(1.05);
}

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

.selected-item {
  background-color: #e3f2fd;
}

.sold-out {
  background-color: #414141;
}
</style>
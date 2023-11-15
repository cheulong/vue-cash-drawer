<script setup>
import CashRegister from './components/CashRegister.vue'
import { ref } from 'vue'

const cid = ref([
  ["ONE", 1],
  ["TWO", 4],
  ["FIVE", 5],
  ["TEN", 60],
  ["TWENTY", 100],
  ["FIFTY", 50],
  ["HUNDRED", 200],
])

const price = ref(25)
const cash = ref(30)
function updateCid(data) {
  console.log({ data });
  cid.value = data
}

</script>

<template>
  <h1>Cash Drawer</h1>
  <CashRegister :price="price" :cash="cash" :cid="cid" @CIDRemain="updateCid" />
  <CashRegister :price="price" :cash="cash" :cid="cid" @CIDRemain="updateCid" />
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}

.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>

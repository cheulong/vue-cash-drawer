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


function updateCid(data) {
  cid.value = data
}

</script>

<template>
  <h1>Cash Drawer</h1>
  <CashRegister :price="25" :cash="30" :cid="cid" @CIDRemain="updateCid" />
  <CashRegister :price="25" :cash="100" :cid="cid" @CIDRemain="updateCid" />
  <CashRegister :price="95" :cash="100" :cid="cid" @CIDRemain="updateCid" />
  <CashRegister :price="660" :cash="1000" :cid="cid" @CIDRemain="updateCid" />
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

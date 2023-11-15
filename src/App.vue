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
const history = ref([])


function updateCid(data) {
  cid.value = data
}
function updateHistory(data) {
  history.value = [...history.value, data]
}
</script>

<template>
  <h1>Cash Drawer</h1>
  <CashRegister :cid="cid" @CIDRemain="updateCid" @updateHistory="updateHistory" />
  <ul>
    <li v-for="item in history">
      <p>Status: {{ item.status }}</p>
      <p>Change: {{ item.change }}</p>
    </li>
  </ul>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  transition: filter 300ms;
}

.logo:hover {
  filter: drop - shadow(0 0 2em #646cffaa);
}

.logo.vue:hover {
  filter: drop - shadow(0 0 2em #42b883aa);
}

ul {
  text-align: left;
}
</style>
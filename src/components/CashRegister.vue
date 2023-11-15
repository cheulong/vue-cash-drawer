<script setup>
import { ref } from 'vue'

const emit = defineEmits(['CIDRemain', 'updateHistory'])
const {

  cid } = defineProps({

    cid: Array
  })

const status = ref('')
const remainChange = ref([])
const changeArray = ref([])
const inputCash = ref(0)
const inputPrice = ref(0)
function handleRemainCID(remainCID, index, returnedAmount) {
  return remainCID.map((c, i) => (i === index ? [c[0], c[1] - returnedAmount] : c));
}
function isInsufficientFunds(totalCID, change) {
  return change > totalCID;
}
function isDrawerClosed(totalCID, change) {
  return totalCID == change;
}

function calculateChange(cid, price, cash) {
  const currencyUnits = [
    { name: "ONE", value: 1 },
    { name: "TWO", value: 2 },
    { name: "FIVE", value: 5 },
    { name: "TEN", value: 10 },
    { name: "TWENTY", value: 20 },
    { name: "FIFTY", value: 50 },
    { name: "HUNDRED", value: 100 },
  ];

  const Change = cash - price;
  let changeArray1 = [];
  let remainChange = Change;
  let remainCID = [...cid]
  let totalCID = 0;

  for (const unit of cid) {
    totalCID += unit[1];
  }

  for (let i = currencyUnits.length - 1; i >= 0; i--) {
    const { name, value } = currencyUnits[i];
    const availableUnit = remainCID.find((item) => item[0] === name);

    if (availableUnit) {
      const maxUnits = Math.floor(availableUnit[1] / value);
      const returnedUnits = Math.min(maxUnits, Math.floor(remainChange / value));
      const returnedAmount = returnedUnits * value;

      if (returnedUnits > 0) {
        changeArray1.push([name, returnedAmount]);
        remainChange = remainChange - returnedAmount;
        remainCID = handleRemainCID(remainCID, i, returnedAmount);
        emit('CIDRemain', remainCID)
      }
    }
  }
  if (isInsufficientFunds(totalCID, Change)) {
    status.value = "INSUFFICIENT_FUNDS";
    changeArray.value = [];

  } else if (isDrawerClosed(totalCID, Change)) {
    status.value = "CLOSED";
    changeArray.value = [...changeArray1];
  } else {
    status.value = "OPEN";
    changeArray.value = [...changeArray1];
  }
  if (changeArray.value.length == 0) {
    status.value = "INSUFFICIENT_FUNDS";
    changeArray.value = [];
  }
  emit('updateHistory', {
    status: status.value,
    change: changeArray.value
  })

}


</script>

<template>
  <h1>{{ msg }}</h1>

  <div>
    <label>Price:
      <input v-on:change="(event) => inputPrice = event.target.value" :value="inputPrice" type="text" /></label>
    <label>Cash: <input v-on:change="(event) => inputCash = event.target.value" :value="inputCash" type="text" /></label>
    <button @click="calculateChange(cid, inputPrice, inputCash)">Pay</button>

  </div>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>

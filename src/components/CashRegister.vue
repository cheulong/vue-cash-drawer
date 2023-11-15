<script setup>
import { ref } from 'vue'

const emit = defineEmits(['CIDRemain'])
const {
  price,
  cash,
  cid } = defineProps({
    price: Number,
    cash: Number,
    cid: Array
  })

const status = ref('')
const remainChange = ref([])
const changeArray = ref([])
function handleRemainCID(remainCID, index, returnedAmount) {
  return remainCID.map((c, i) => (i === index ? [c[0], c[1] - returnedAmount] : c));
}
function isInsufficientFunds(totalCID, change) {
  return change > totalCID;
}
function isDrawerClosed(totalCID, change) {
  return totalCID == change;
}

function calculateChange(cid, cash, price) {
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
  } else if (isDrawerClosed(totalCID, Change)) {
    status.value = "CLOSED";
    changeArray.value = [...changeArray1];
  } else {
    status.value = "OPEN";
    changeArray.value = [...changeArray1];
  }
  console.log(remainCID);
}

calculateChange(cid, cash, price)

</script>

<template>
  <h1>{{ msg }}</h1>

  <div>
    <div v-if="status === 'INSUFFICIENT_FUNDS'">
      <p>Status: INSUFFICIENT_FUNDS</p>
      <p>Change: []</p>
    </div>
    <div v-else-if="status === 'CLOSED'">
      <p>Status: CLOSED</p>
      <p>Change: {{ JSON.stringify(changeArray) }}</p>
    </div>
    <div v-else>
      <p>Status: OPEN</p>
      <p>Change: {{ JSON.stringify(changeArray) }}</p>
    </div>
  </div>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>

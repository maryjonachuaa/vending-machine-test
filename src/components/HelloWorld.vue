<script setup>
import { ref } from 'vue'

// state
const bill = ref(0);
const owed = ref(0);
const changeTotal = ref([]);


// function get change
const getChange = (bill, owed) => {
      const denominations = [1000, 500, 200, 100, 50, 20, 10, 5, 1];
      let change = [];
      let remaining = bill - owed;

      for (let denomination of denominations) {
        let count = Math.floor(remaining / denomination);
        if (count > 0) {
          change.push({ denomination: `P${denomination}`, count: count });
          remaining -= count * denomination;
        }
      }

      return result;
    };



</script>

<template>
     <div>
    <h1>Change Calculator</h1>
    <div>
      <label for="bill">Bill Amount:</label>
      <input type="number" v-model="bill" id="bill" />
    </div>
    <div>
      <label for="owed">Owed Amount:</label>
      <input type="number" v-model="owed" id="owed" />
    </div>
    <button @click="calculateChange">Calculate Change</button>
    <div v-if="change.length > 0">
      <h2>Change:</h2>
      <ul>
        <li v-for="(item, index) in change" :key="index">
          {{ item.denomination }}: {{ item.count }}
        </li>
      </ul>
    </div>
  </div>
 
</template>



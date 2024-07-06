<script setup>
import { ref } from "vue";

// state
const bill = ref(0);
const owed = ref(0);
const changeTotal = ref([]);
const noChange = ref(false);
const showChangeDetails = ref(false);

// function get change
const getChange = (bill, owed) => {
  const denominations = [1000, 500, 200, 100, 50, 20, 10, 5, 1];
  let change = [];
  let remaining = bill - owed;

  for (let denomination of denominations) {
    let count = Math.floor(remaining / denomination);
    if (count > 0) {
      change.push({ denomination: denomination, count: count });
      remaining -= count * denomination;
    }
  }

  return change;
};

const onSubmit = () => {
  changeTotal.value = getChange(bill.value, owed.value);
  noChange.value = bill.value - owed.value === 0;
  showChangeDetails.value = true;

  // bill - owed != negative
  if (changeTotal.value === null) {
    showChangeDetails.value = false;
    noChange.value = false;
  }
};

</script>

<template>
  <div
    class="flex flex-col justify-center items-center h-screen bg-gradient-to-r from-indigo-400 to-cyan-400"
  >
    <div class="p-8 shadow-lg rounded-lg bg-gray-100 m-auto md:w-1/2">
      <h1 class="text-4xl text-center font-semibold">Change Calculator</h1>
      <div class="flex gap-3 flex-col md:flex-row mt-10">
        <!-- Bill Amount -->
        <div class="w-full">
          <label for="bill" class="font-semibold">Bill Amount:</label>
          <input
            type="number"
            v-model="bill"
            id="bill"
            class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5"
            required
          />
        </div>
        <!-- Owed Amount -->
        <div class="w-full">
          <label for="owed" class="font-semibold">Owed Amount:</label>
          <input
            type="number"
            v-model="owed"
            id="owed"
            class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5"
            required
          />
        </div>
      </div>
      <button
        @click="onSubmit"
        class="mt-4 bg-blue-800 text-white p-2 rounded-lg w-full hover:bg-blue-700"
      >
        Calculate Change
      </button>

      <!-- Change Denomination Container -->
      <div class="mt-4" v-if="showChangeDetails">
        <h2 class="text-lg mt-9 font-semibold">Total Change:</h2>

        <!-- container -->

        <div v-if="changeTotal.length !== 0" class="mt-5">
          <div
            class="grid grid-cols-2 bg-gray-200 p-2 font-semibold mt-2 rounded-md"
          >
            <div class="text-center">Qty</div>

            <div class="text-center">Denomination</div>
          </div>
        </div>
        <!-- --------- -->
        <div v-if="changeTotal.length !== 0" class="mt-1 overflow-y-auto h-28">
          <div
            v-for="(c, index) in changeTotal"
            :key="index"
            class="grid grid-cols-2 p-2 border mt-2 rounded-md"
          >
            <div class="text-center">
              {{ c.count }}
            </div>

            <div class="text-center">
              P {{ c.denomination }}
              <span v-if="c.denomination > 5">bill</span>
              <span v-if="c.denomination <= 5">coin</span>
            </div>
          </div>
        </div>
        <div v-else>
          <p
            v-if="noChange"
            class="text-center p-4 font-medium text-green-600 text-lg"
          >
            No Change Needed.
          </p>
          <p v-else class="text-center p-4 text-red-600 font-medium text-lg">
            Insufficient Bill Amount.
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const { transactions } = defineProps({
  transactions: null,
})

const totalIncome = computed(() => {
  return transactions
    .filter((t) => t.type === 'income')
    .map((t) => t.value)
    .reduce((acc, v) => acc + v, 0)
})

const totalExpenses = computed(() => {
  return transactions
    .filter((t) => t.type === 'expense')
    .map((t) => t.value)
    .reduce((acc, v) => acc + v, 0)
})

const balance = computed(() => {
  return totalIncome.value - totalExpenses.value
})

const negative = computed(() => balance.value < 0)
</script>

<template>
  <div class="bg-white shadow rounded-xl p-4 text-center">
    <h2 class="text-lg font-semibold mb-3">Сводка</h2>

    <div class="grid grid-cols-3 gap-4">
      <div>
        <p class="text-gray-500">Доходы</p>
        <p class="text-green-600 text-xl font-semibold">
          {{ totalIncome }}
        </p>
      </div>
      <div>
        <p class="text-gray-500">Расходы</p>
        <p class="text-red-600 text-xl font-semibold">
          {{ totalExpenses }}
        </p>
      </div>
      <div>
        <p class="text-gray-500">Баланс</p>
        <p class="text-gray-800 text-xl font-semibold">
          {{ balance }}
        </p>
      </div>
    </div>

    <p v-if="negative" class="mt-4 text-red-500 font-medium">⚠️ Ваш баланс отрицательный!</p>
  </div>
</template>

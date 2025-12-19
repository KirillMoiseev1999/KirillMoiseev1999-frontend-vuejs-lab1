<script setup lang="ts">
import { ref, watch } from 'vue'

import BudgetForm from './BudgetForm.vue'
import BudgetTable from './BudgetTable.vue'

const transactions = ref(JSON.parse(localStorage.getItem('transactions')) ?? [])

watch(
  transactions,
  (newTransactions) => {
    localStorage.setItem('transactions', JSON.stringify(newTransactions))
  },
  {
    deep: true,
  },
)

function add(transation) {
  transactions.value.push(transation)
}

function remove(id) {
  transactions.value = transactions.value.filter((transation) => transation.id !== id)
}
</script>

<template>
  <div class="min-h-screen bg-gray-50 text-gray-900 p-6">
    <h1 class="text-3xl font-bold mb-6 text-center">Калькулятор бюджета</h1>

    <div class="max-w-3xl mx-auto space-y-8">
      <BudgetForm @add="add" />
      <BudgetTable :transactions="transactions" @remove="remove" />
    </div>
  </div>
</template>

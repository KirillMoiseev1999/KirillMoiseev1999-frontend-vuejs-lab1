<script setup>
import { ref, computed } from 'vue'
import BudgetSummary from './BudgetSummary.vue'

defineEmits({
  remove: null,
})

const { transactions } = defineProps({
  transactions: null,
})

const sorting = ref('no') // 'no', 'desc', 'asc'

function switchSorting() {
  switch (sorting.value) {
    case 'no':
      sorting.value = 'desc' // первый клик — убывание
      break
    case 'desc':
      sorting.value = 'asc' // второй — возрастание
      break
    case 'asc':
      sorting.value = 'no' // третий — сброс
      break
  }
}

const sortedTransactions = computed(() => {
  const newTransactions = [...transactions]
  if (sorting.value === 'asc') {
    // по возрастанию: от малого к большому
    newTransactions.sort(
      (a, b) =>
        (a.type === 'income' ? a.value : -a.value) - (b.type === 'income' ? b.value : -b.value),
    )
  }
  if (sorting.value === 'desc') {
    // по убыванию: от большого к малому
    newTransactions.sort(
      (a, b) =>
        (b.type === 'income' ? b.value : -b.value) - (a.type === 'income' ? a.value : -a.value),
    )
  }
  return newTransactions
})

// Функция для отображения значка
function getSortSymbol() {
  switch (sorting.value) {
    case 'desc':
      return ' ↑'
    case 'asc':
      return ' ↓'
    default:
      return ' —'
  }
}
</script>

<template>
  <div class="bg-white shadow rounded-xl p-4">
    <h2 class="text-lg font-semibold mb-3">Операции</h2>

    <table class="w-full text-left border-collapse">
      <thead>
        <tr class="border-b">
          <th class="p-2">Название</th>
          <th class="p-2">Тип</th>
          <th class="p-2">
            <button @click="switchSorting" class="font-medium hover:underline">
              Сумма{{ getSortSymbol() }}
            </button>
          </th>
          <th class="p-2">Действия</th>
        </tr>
      </thead>
      <tbody>
        <tr class="border-b" v-for="transation in sortedTransactions" :key="transation.id">
          <td class="p-2">{{ transation.name }}</td>

          <td class="p-2 text-green-600 font-medium" v-if="transation.type === 'income'">Доход</td>
          <td class="p-2 text-red-600 font-medium" v-else>Расход</td>

          <td class="p-2" v-if="transation.type === 'income'">+{{ transation.value }}</td>
          <td class="p-2" v-else>-{{ transation.value }}</td>

          <td class="p-2 text-sm text-red-500 cursor-pointer">
            <button @click="$emit('remove', transation.id)">Удалить</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>

  <BudgetSummary :transactions="transactions" />
</template>

<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      v-for="transaction in transactions"
      :key="transaction.id"
      :class="transaction.amount < 0 ? 'minus' : 'plus'"
    >
      {{ transaction.text }} <span>${{ transaction.amount }}</span
      ><button @click="deleteTransaction(transaction.id)" class="delete-btn">
        x
      </button>
    </li>
  </ul>
</template>

<script setup lang="ts">
import { defineProps, defineEmits } from 'vue'
import type { ITransaction } from '@/App.vue'

interface Props {
  transactions: ITransaction[]
}
const props = defineProps<Props>()

const emit = defineEmits<{ 'transaction-deleted': [id: number] }>()

const deleteTransaction = (id: number) => {
  emit('transaction-deleted', id)
}
</script>

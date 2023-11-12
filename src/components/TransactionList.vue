<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      v-for="transaction in transactions"
      :key="transaction.id"
      :class="transaction.amount < 0 ? 'minus' : 'plus'"
    >
      {{ transaction.text }} <span>${{ transaction.amount }}</span
      ><button class="delete-btn">x</button>
    </li>
  </ul>
</template>

<script setup lang="ts">
import { defineProps } from 'vue'
import type { ITransaction } from '@/App.vue'

interface Props {
  transactions: ITransaction[]
}
const props = defineProps<Props>()
</script>

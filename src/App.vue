<script setup lang="ts">
import Header from './components/Header.vue'
import Balance from './components/Balance.vue'
import IncomeExpenses from './components/IncomeExpenses.vue'
import TransactionList from './components/TransactionList.vue'
import AddTransaction, {
  type TransactionSubmitted,
} from './components/AddTransaction.vue'
import { useToast } from 'vue-toastification'

export interface ITransaction {
  id: number
  text: string
  amount: number
}

import { reactive, computed } from 'vue'

const toast = useToast()

const transactions = reactive<ITransaction[]>([
  { id: 1, text: 'Flower', amount: -19.99 },
  { id: 2, text: 'Salary', amount: 299.97 },
  { id: 3, text: 'Book', amount: -10 },
  { id: 4, text: 'Camera', amount: 150 },
])

const total = computed(() => {
  return transactions.reduce((acc, currVal) => {
    return acc + currVal.amount
  }, 0)
})

const income = computed(() => {
  return transactions.reduce((acc, currVal) => {
    return currVal.amount > 0 ? acc + currVal.amount : acc
  }, 0)
})

const expanse = computed(() => {
  return transactions.reduce((acc, currVal) => {
    return currVal.amount < 0 ? acc + currVal.amount : acc
  }, 0)
})

const handleTransactionSubmitted = (transactionData: TransactionSubmitted) => {
  transactions.push({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount,
  })

  toast.success('Transaction added')
}

const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000)
}
</script>

<template>
  <Header />
  <div class="container">
    <Balance :total="total" />
    <IncomeExpenses :income="income" :expense="expanse" />
    <TransactionList :transactions="transactions" />
    <AddTransaction @transaction-submitted="handleTransactionSubmitted" />
  </div>
</template>

<style scoped></style>

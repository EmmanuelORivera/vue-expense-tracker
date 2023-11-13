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

import { reactive, computed, onMounted } from 'vue'

const toast = useToast()

const transactions = reactive<ITransaction[]>([])

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

  saveTransactionsToLocalStorage()
  toast.success('Transaction added')
}

const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000)
}

const handleTransactionDeleted = (id: number) => {
  const index = transactions.findIndex((transaction) => transaction.id === id)
  if (index !== -1) {
    transactions.splice(index, 1)
    saveTransactionsToLocalStorage()
    toast.success(`Transaction ${id} was deleted`)
  }
}

const saveTransactionsToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions))
}

onMounted(() => {
  const savedTransactionString = localStorage.getItem('transactions')
  if (savedTransactionString !== null) {
    const savedTransactions: ITransaction[] = JSON.parse(savedTransactionString)
    transactions.push(...savedTransactions)
  }
})
</script>

<template>
  <Header />
  <div class="container">
    <Balance :total="total" />
    <IncomeExpenses :income="income" :expense="expanse" />
    <TransactionList
      :transactions="transactions"
      @transaction-deleted="handleTransactionDeleted"
    />
    <AddTransaction @transaction-submitted="handleTransactionSubmitted" />
  </div>
</template>

<style scoped></style>

<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="handleSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input type="text" id="text" placeholder="Enter text..." v-model="text" />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />
        (negative - expense, positive - income)</label
      >
      <input
        type="number"
        id="amount"
        placeholder="Enter amount..."
        v-model="amount"
      />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>

<script setup lang="ts">
import { ref, defineEmits } from 'vue'
import { useToast } from 'vue-toastification'

export interface TransactionSubmitted {
  text: string
  amount: number
}

const text = ref('')
const amount = ref('')

const emit = defineEmits<{
  'transaction-submitted': [TransactionSubmitted]
}>()

const toast = useToast()

const handleSubmit = () => {
  if (!text.value || !amount.value) {
    toast.error('Both fields must be filled')
    return
  }

  const transactionData: TransactionSubmitted = {
    text: text.value,
    amount: parseFloat(amount.value),
  }

  emit('transaction-submitted', transactionData)
}
</script>

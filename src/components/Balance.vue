<template>
  <div>
    <h4>Your Balance</h4>
    <h1>${{ total }}</h1>
  </div>
</template>

<script setup lang="ts">
import { defineProps } from 'vue'

interface Props {
  total: number
}
const props = defineProps<Props>()
</script>

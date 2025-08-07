<template>
    <h3>History</h3>
    <ul id="list" class="list">
        <li v-for="t in transactions" :class="t.amount > 0 ? 'plus' : 'minus'">
            {{ t.text }} <span>${{ t.amount }}</span><button class="delete-btn" @click="handleDelete(t.id)">x</button>
        </li>
    </ul>
</template>

<script setup>
import { defineProps, defineEmits } from 'vue';
const props = defineProps({
    transactions: { type: Array, required: true }
})
const emit = defineEmits(['transactionDelete'])
const handleDelete = (id) => {
    emit('transactionDelete', id)
}
</script>
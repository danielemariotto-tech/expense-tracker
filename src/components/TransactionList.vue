<template>
    <h3>History</h3>
    <ul id="list" class="list">
        <li v-for="t in transactions" :class="t.amount > 0 ? 'plus' : 'minus'">
            {{ t.text }} | {{ t.category }} {{ t.isRecurring ? ' | Recurring' : '' }}
            <span>${{ t.amount }}</span>
            <button class="delete-btn" @click="handleDelete(t.id)">x</button>
            <button class="edit-btn" @click="handleEdit(t.id)">Edit</button>
        </li>
    </ul>
</template>

<script setup>
import { defineProps, defineEmits } from 'vue';
const props = defineProps({
    transactions: { type: Array, required: true }
})
const emit = defineEmits(['transactionDelete', 'transactionEdit'])
const handleDelete = (id) => {
    emit('transactionDelete', id)
}
const handleEdit = (id) => {
    emit('transactionEdit', id)
}
</script>
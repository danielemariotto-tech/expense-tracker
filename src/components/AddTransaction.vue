<template>
    <h3>Add new transaction</h3>
    <form id="form" @submit.prevent="onSubmit">
        <div class="form-control">
            <label for="text">Text</label>
            <input type="text" id="text" v-model="text" placeholder="Enter text..." />
        </div>
        <div class="form-control">
            <label for="amount">Amount <br />
                (negative - expense, positive - income)</label>
            <input type="text" id="amount" v-model="amount" placeholder="Enter amount..." />
        </div>
        <button class="btn">Add transaction</button>
    </form>
</template>

<script setup>
import { ref, defineEmits } from 'vue'
import { useToast } from 'vue-toastification';
const emits = defineEmits(['transactionSended'])
const toast = useToast()
const onSubmit = () => {
    if (!text.value || !amount.value) {


        toast.error("Both fields must be filled")
        return
    }
    const transaction = { text: text.value, amount: parseFloat(amount.value) }
    emits('transactionSended', transaction)

    text.value = ''
    amount.value = ''
}

const text = ref('')
const amount = ref('')
</script>
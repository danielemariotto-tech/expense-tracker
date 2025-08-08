<template>
    <Header />

    <div class="container">
        <Balance :total="total"></Balance>
        <IncomeExpenses :income="income" :expenses="expenses"></IncomeExpenses>
        <TransactionList @transactionDelete="handleTransactionDelete" @transactionEdit="handleTransactionEdit"
            :transactions="transactions"></TransactionList>
        <AddEditTransaction @transactionSended="handleTransactionSended" :editTransaction="editTransaction">
        </AddEditTransaction>
        <RecurringExpensesIncomes :transactions="recurringTransactions" />
    </div>
</template>


<script setup>
import { ref, computed, onMounted } from 'vue'

import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';
import TransactionList from './components/TransactionList.vue';
import AddEditTransaction from './components/AddEditTransaction.vue'; // Cambia nome qui
import RecurringExpensesIncomes from './components/RecurringExpensesIncomes.vue';

const transactions = ref([])
const editTransaction = ref(null)

onMounted(() => {
    const transactionsFromStorage = JSON.parse(localStorage.getItem('transactions'))
    if (transactionsFromStorage) {
        transactions.value = transactionsFromStorage
    }
})

const total = computed(() => transactions.value.reduce((acc, t) => acc + t.amount, 0))
const income = computed(() => transactions.value.filter(t => t.amount > 0).reduce((acc, t) => acc + t.amount, 0).toFixed(2))
const expenses = computed(() => transactions.value.filter(t => t.amount < 0).reduce((acc, t) => acc + t.amount, 0).toFixed(2))

const generateID = () => {
    let id
    if (transactions.value.length == 0) {
        id = 1
    } else {
        id = transactions.value[transactions.value.length - 1].id + 1
    }
    return id
}

const handleTransactionSended = (transactionData) => {
    if (transactionData.id) {
        // Edit mode: update existing transaction
        const idx = transactions.value.findIndex(t => t.id === transactionData.id)
        if (idx !== -1) {
            transactions.value[idx] = { ...transactionData }
        }
        editTransaction.value = null
    } else {
        // Add mode: add new transaction
        transactions.value.push({
            id: generateID(),
            text: transactionData.text,
            amount: transactionData.amount,
            category: transactionData.category,
            isRecurring: transactionData.isRecurring || false,
            dayOfMonth: transactionData.isRecurring ? transactionData.dayOfMonth : null,
            frequency: transactionData.isRecurring ? transactionData.frequency : null
        })
    }
    saveTransactionsToLocalStorage()
}

const handleTransactionDelete = (id) => {
    transactions.value = transactions.value.filter(t => t.id != id)
    saveTransactionsToLocalStorage()
}

const handleTransactionEdit = (id) => {
    const transaction = transactions.value.find(t => t.id === id)
    if (transaction) {
        editTransaction.value = { ...transaction }
    }
}

const saveTransactionsToLocalStorage = () => {
    localStorage.setItem('transactions', JSON.stringify(transactions.value))
}

const recurringTransactions = computed(() =>
    transactions.value.filter(t => t.isRecurring === true)
)
</script>
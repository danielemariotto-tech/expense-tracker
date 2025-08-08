<template>
    <Header />

    <div class="container">
        <Balance :total="total"></Balance>
        <IncomeExpenses :income="income" :expenses="expenses"></IncomeExpenses>
        <TransactionList @transactionDelete="handleTransactionDelete" :transactions="transactions"></TransactionList>
        <AddTransaction @transactionSended="handleTransactionSended"></AddTransaction>
        <RecurringExpensesIncomes :transactions="recurringTransactions" />
    </div>
</template>


<script setup>
import { ref, computed, onMounted } from 'vue'

import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';
import RecurringExpensesIncomes from './components/RecurringExpensesIncomes.vue';

onMounted(() => {
    const transactionsFromStorage = JSON.parse(localStorage.getItem('transactions'))

    if (transactionsFromStorage) {
        transactions.value = transactionsFromStorage
    }
})

const transactions = ref([])
const total = computed(() => transactions.value.reduce((acc, t) => acc + t.amount, 0))
const income = computed(() => transactions.value.filter(t => t.amount > 0).reduce((acc, t) => acc + t.amount, 0).toFixed(2))
const expenses = computed(() => transactions.value.filter(t => t.amount < 0).reduce((acc, t) => acc + t.amount, 0).toFixed(2))

const generateID = () => {
    console.log(transactions.value.length);
    let id
    if (transactions.value.length == 0) {
        id = 1
    }

    else {

        id = transactions.value[transactions.value.length - 1].id + 1
    }
    return id

}
const handleTransactionSended = (transactionData) => {

    transactions.value.push({
        id: generateID(),
        text: transactionData.text,
        amount: transactionData.amount,
        category: transactionData.category,
        isRecurring: transactionData.isRecurring || false,
        dayOfMonth: transactionData.isRecurring ? transactionData.dayOfMonth : null,
        frequency: transactionData.isRecurring ? transactionData.frequency : null
    })
    saveTransactionsToLocalStorage()

}

const handleTransactionDelete = (id) => {
    transactions.value = transactions.value.filter(t => t.id != id)
    saveTransactionsToLocalStorage()
}

const saveTransactionsToLocalStorage = () => {
    localStorage.setItem('transactions', JSON.stringify(transactions.value))
}

const recurringTransactions = computed(() =>
    transactions.value.filter(t => t.isRecurring === true)
)
</script>
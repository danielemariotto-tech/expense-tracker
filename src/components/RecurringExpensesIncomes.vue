<template>
    <div style="display: flex; gap: 2rem;">
        <div style="flex: 1;">
            <h3>Recurring Expenses</h3>
            <ul>
                <li v-for="expense in recurringExpenses" :key="expense.id">
                    {{ expense.text }} ${{ expense.amount }}
                    (Day: {{ expense.dayOfMonth }}, Frequency: {{ expense.frequency }})
                </li>
            </ul>
        </div>
        <div style="flex: 1;">
            <h3>Recurring Incomes</h3>
            <ul>
                <li v-for="income in recurringIncomes" :key="income.id">
                    {{ income.text }} ${{ income.amount }}
                    (Day: {{ income.dayOfMonth }}, Frequency: {{ income.frequency }})
                </li>
            </ul>
        </div>
    </div>
</template>
<script setup>
import { computed } from 'vue'

const props = defineProps({
    transactions: {
        type: Array,
        default: () => []
    }
})


const recurringIncomes = computed(() =>
    props.transactions.filter(t => t.amount > 0)
)
const recurringExpenses = computed(() =>
    props.transactions.filter(t => t.amount < 0)
)

</script>
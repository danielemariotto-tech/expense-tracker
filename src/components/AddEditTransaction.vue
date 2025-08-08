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
        <div class="form-control">
            <label for="category">Category</label>
            <br>
            <select v-model="category">
                <option v-for="i in categories" :key="i">{{ i }}</option>
            </select>
        </div>
        <div class="form-control">
            <label>
                <input type="checkbox" v-model="isRecurring" />
                Recurring
            </label>
        </div>
        <div v-if="isRecurring" class="form-control">
            <label for="dayOfMonth">Day of Month</label>
            <input type="number" id="dayOfMonth" v-model="dayOfMonth" min="1" max="31" placeholder="Enter day..." />
        </div>
        <div v-if="isRecurring" class="form-control">
            <label for="frequency">Frequency</label>
            <select id="frequency" v-model="frequency">
                <option value="Monthly">Monthly</option>
            </select>
        </div>
        <button class="btn">{{editingMode ? 'Edit' : 'Add'}} transaction</button>
    </form>
</template>

<script setup>
import { ref, defineEmits, watch, defineProps } from 'vue'
import { useToast } from 'vue-toastification';

const emits = defineEmits(['transactionSended'])
const toast = useToast()

const props = defineProps({
    editTransaction: {
        type: Object,
        default: null
    }
})

const text = ref('')
const amount = ref('')
const category = ref('')
const isRecurring = ref(false)
const dayOfMonth = ref('')
const frequency = ref('Monthly')
const editingMode = ref(false)


const categories = ref([
    "Rent",
    "Mortgage",
    "Property Tax",
    "Home Insurance",
    "Repairs & Maintenance",
    "Electricity",
    "Gas",
    "Water",
    "Internet",
    "Trash & Recycling",
    "Fuel / Gas",
    "Car Loan",
    "Car Insurance",
    "Public Transport",
    "Ride-Sharing",
    "Vehicle Maintenance",
    "Parking Fees",
    "Tolls",
    "Groceries",
    "Restaurants",
    "Coffee Shops",
    "Fast Food",
    "Meal Delivery",
    "Mobile Phone",
    "Streaming Subscriptions",
    "Clothing",
    "Shoes",
    "Accessories",
    "Electronics",
    "Personal Care",
    "Cosmetics & Toiletries",
    "Health Insurance",
    "Doctor Visits",
    "Medications",
    "Dental Care",
    "Vision Care",
    "Mental Health Services",
    "Gym Memberships",
    "Childcare",
    "School Fees",
    "Tuition",
    "Books & Supplies",
    "Toys",
    "Allowance",
    "Flights",
    "Hotels",
    "Vacation Packages",
    "Travel Insurance",
    "Souvenirs",
    "Local Transportation",
    "Movies",
    "Concerts & Events",
    "Hobbies",
    "Games & Apps",
    "Nightlife / Bars",
    "Birthday Gifts",
    "Wedding Gifts",
    "Charity Donations",
    "Religious Contributions",
    "Savings Contributions",
    "Investments",
    "Retirement Fund",
    "Loan Payments",
    "Credit Card Payments",
    "Bank Fees",
    "Pet Expenses",
    "Legal Fees",
    "Education",
    "Business Expenses",
    "Emergency Fund",
    "Unplanned Purchases",
    "Miscellaneous"
])

// Watch for changes to editTransaction and populate form fields
watch(
    () => props.editTransaction,
    (newVal) => {
        if (newVal) {
            editingMode.value = true
            text.value = newVal.text || ''
            amount.value = newVal.amount !== undefined ? newVal.amount : ''
            category.value = newVal.category || ''
            isRecurring.value = newVal.isRecurring || false
            dayOfMonth.value = newVal.dayOfMonth !== undefined && newVal.dayOfMonth !== null ? newVal.dayOfMonth : ''
            frequency.value = newVal.frequency || 'Monthly'
        } else {
            text.value = ''
            amount.value = ''
            category.value = ''
            isRecurring.value = false
            dayOfMonth.value = ''
            frequency.value = 'Monthly'
        }
    },
    { immediate: true }
)

const onSubmit = () => {
    if (!text.value || !amount.value || !category.value) {
        toast.error("All fields must be filled")
        return
    }
    if (isRecurring.value && (!dayOfMonth.value || !frequency.value)) {
        toast.error("Please provide day of month and frequency for recurring transaction")
        return
    }
    const transaction = {
        id: props.editTransaction?.id, // preserve id if editing
        text: text.value,
        amount: parseFloat(amount.value),
        category: category.value,
        isRecurring: isRecurring.value,
        dayOfMonth: isRecurring.value ? parseInt(dayOfMonth.value) : null,
        frequency: isRecurring.value ? frequency.value : null
    }
    emits('transactionSended', transaction)

    text.value = ''
    amount.value = ''
    category.value = ''
    isRecurring.value = false
    dayOfMonth.value = ''
    frequency.value = 'Monthly'
    editingMode.value = false
}
</script>
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
                <option v-for="i in categories">{{ i }}</option>
            </select>
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
    if (!text.value || !amount.value || !category.value) {


        toast.error("All fields must be filled")
        return
    }
    const transaction = { text: text.value, amount: parseFloat(amount.value), category: category.value }
    emits('transactionSended', transaction)

    text.value = ''
    amount.value = ''
    category.value = ''
}

const text = ref('')
const amount = ref('')
const category = ref('')
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
]
)
</script>
<template>
    <h3>Add new transaction</h3>
    <form id="form" @submit.prevent="onSubmit">
        <div class="form-control">
            <label for="text">Text</label>
            <input type="text" id="text" placeholder="Enter text..." v-model="text" />
        </div>
        <div class="form-control">
            <label for="amount">Amount <br />
                (negative - expense, positive - income)</label>
            <input type="text" id="amount" placeholder="Enter amount..." v-model="amount" />
        </div>
        <button class="btn">Add transaction</button>
    </form>
</template>

<script setup>
import { ref } from 'vue'
import { useToast } from "vue-toastification";

// Get toast interface
const toast = useToast();

// Emits
const emit = defineEmits(['transactionSubmitted'])

const text = ref('')
const amount = ref('')

const onSubmit = () => {

    // check if the values are empty
    if (text.value === '' || amount.value === '') {
        toast.error('Please fill both the fields');
        return;
    }

    // check if the amount value is nan or not a number
    if (isNaN(+amount.value)) {
        toast.error('Please enter a valid amount');
        return;
    }

    // Emitting the event
    emit('transactionSubmitted', {
        text: text.value,
        amount: +amount.value
    })

    // clearing the values
    text.value = ''
    amount.value = ''
}

</script>
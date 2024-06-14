<template>
  <Header />

  <div class="container">

    <Balance :total="+total" />
    <IncomeExpenses :income="+income" :expenses="+expenses" />
    <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted" />
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />

  </div>

</template>

<script setup>
import { ref, computed, onMounted } from 'vue';
import { useToast } from 'vue-toastification'
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";

// Checking for data on localstorage and adding to the transaction if exists when mounted

onMounted(() => {
  if (localStorage.getItem('transactions')) {
    transactions.value = JSON.parse(localStorage.getItem('transactions'));
  }
});

// Inntilize toast
const toast = useToast();

// List of transactions
const transactions = ref([]);

// Total Balance
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
});

// Income
const income = computed(() => {
  return transactions.value.filter((transaction) => transaction.amount > 0).reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0).toFixed(2);
});

// Expenses
const expenses = computed(() => {
  return transactions.value.filter((transaction) => transaction.amount < 0).reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0).toFixed(2);;
});

// Handle transaction submitted
const handleTransactionSubmitted = (transaction) => {

  // Add to transactions
  transactions.value.push({
    id: generateId(),
    text: transaction.text,
    amount: transaction.amount,
  });

  // saveToLocalStorage
  saveToLocalStorage();

  // Show success message
  toast.success('Transaction added successfully');

};

// generate unique id for each transaction
const generateId = () => {
  return Math.floor(Math.random() * 1000000);
};

// Deleting transcation
const handleTransactionDeleted = (id) => {
  transactions.value = transactions.value.filter((transaction) => transaction.id !== id);

  // saveToLocalStorage
  saveToLocalStorage()

  toast.error('Transaction deleted successfully');
};

// Function to save to local storage
const saveToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value))
}

</script>
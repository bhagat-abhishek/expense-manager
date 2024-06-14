<template>
  <Header />

  <div class="container">

    <Balance :total="total" />
    <IncomeExpenses :income="+income" :expenses="+expenses" />
    <TransactionList :transactions="transactions" />
    <AddTransaction />

  </div>

</template>

<script setup>
import { ref, computed } from 'vue';
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";

// List of transactions
const transactions = ref([
  { id: 1, text: 'Flower', amount: -20, },
  { id: 2, text: 'Income', amount: 12, },
  { id: 3, text: 'Tickets', amount: -500, },
  { id: 4, text: 'Salary', amount: 652, },
]);

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

</script>
<template>
  <Header />

  <div class="container">

    <Balance />
    <IncomeExpenses />
    <TransactionList :transactions="transactions" />
    <AddTransaction />

  </div>

</template>

<script setup>
import { ref } from 'vue';
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";

const transactions = ref([
  { id: 1, text: 'Flower', amount: -20, },
  { id: 2, text: 'Income', amount: 12, },
  { id: 3, text: 'Tickets', amount: -500, },
]);

</script>
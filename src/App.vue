<script>
import { ref, onMounted } from 'vue';
import AddTransaction from './components/AddTransaction.vue';
import TransactionList from './components/TransactionList.vue';

export default {
  components: {
    AddTransaction,
    TransactionList
  },
  setup() {
    const transactions = ref([]);

    onMounted(() => {
      const savedTransactions = JSON.parse(localStorage.getItem('transactions'));
      if (savedTransactions) transactions.value = savedTransactions;
    });

    return {
      transactions
    };
  }
};
</script>

<template>
  <div id="app">
    <AddTransaction />
    <TransactionList :transactions="transactions" />
  </div>
</template>

<style>
.table th, .table td {
  text-align: center;
}

.text-success {
  color: green;
}

.text-danger {
  color: red;
}

.fw-bold {
  font-weight: bold;
}
</style>

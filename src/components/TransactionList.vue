<script>
  import { ref, computed, watch } from 'vue';
  
  export default {
    props: {
      transactions: Array
    },
    setup(props) {
      const filter = ref('all');
  
      const filteredTransactions = computed(() => {
        if (filter.value === 'income') {
          return props.transactions.filter(t => t.type === 'Income');
        } else if (filter.value === 'expense') {
          return props.transactions.filter(t => t.type === 'Expense');
        }
        return props.transactions;
      });
  
      const deleteTransaction = (id) => {
        const index = props.transactions.findIndex(t => t.id === id);
        if (index !== -1) {
          props.transactions.splice(index, 1);
          localStorage.setItem('transactions', JSON.stringify(props.transactions));
        }
      };
  
      const amountClass = (amount, type) => {
        let classes = '';
        if (type === 'Income') classes += 'text-success ';
        if (type === 'Expense') classes += 'text-danger ';
        if (amount >= 500) classes += 'fw-bold ';
        return classes;
      };
  
      watch(() => props.transactions, () => {
        localStorage.setItem('transactions', JSON.stringify(props.transactions));
      }, { deep: true });
  
      return {
        filter,
        filteredTransactions,
        deleteTransaction,
        amountClass
      };
    }
  };
</script>

<template>
    <div>
      <div class="select-container">
        <select v-model="filter" @change="applyFilter">
          <option value="all">All</option>
          <option value="income">Income</option>
          <option value="expense">Expense</option>
        </select>
      </div>
      <table class="table">
        <thead>
          <tr>
            <th>Title</th>
            <th>Amount</th>
            <th>Type</th>
            <th>Action</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="transaction in filteredTransactions" :key="transaction.id">
            <td>{{ transaction.title }}</td>
            <td :class="amountClass(transaction.amount, transaction.type)">{{ transaction.amount }}</td>
            <td>{{ transaction.type.toUpperCase() }}</td>
            <td><button @click="deleteTransaction(transaction.id)">Delete</button></td>
          </tr>
        </tbody>
      </table>
      <p v-if="filteredTransactions.length === 0" class="no-transactions">No transactions recorded yet.</p>
    </div>
  </template>
  
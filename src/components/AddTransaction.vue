<script>
  import { ref } from 'vue';
  
  export default {
    setup() {
      const title = ref('');
      const amount = ref(0);
      const type = ref('Income');
  
      const addTransaction = () => {
        if (amount.value <= 0) {
          alert('Amount must be greater than 0');
          return;
        }
        
        const transactions = JSON.parse(localStorage.getItem('transactions')) || [];
        const newTransaction = {
          id: Date.now(),
          title: title.value,
          amount: parseFloat(amount.value),
          type: type.value
        };
        
        transactions.push(newTransaction);
        localStorage.setItem('transactions', JSON.stringify(transactions));
        
        title.value = '';
        amount.value = 0;
        type.value = 'Income';
      };
  
      return {
        title,
        amount,
        type,
        addTransaction
      };
    }
  };
</script>

<template>
    <form @submit.prevent="addTransaction">
      <div class="form-container">
        <div>
        <label for="title">Title</label>
        <input type="text" v-model="title" required />
      </div>
      <div>
        <label for="amount">Amount</label>
        <input type="number" v-model="amount" min="0.01" step="0.01" required />
      </div>
      </div>
      <div class="form-container">
        <div>
        <label for="type">Type</label>
        <select v-model="type" required>
          <option value="Income">Income</option>
          <option value="Expense">Expense</option>
        </select>
      </div>
      <button type="submit">Add</button>
      </div>
    </form>
  </template>
  
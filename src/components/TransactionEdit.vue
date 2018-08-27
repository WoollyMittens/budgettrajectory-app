<template>
<section class="transaction-edit">
  <div class="form-row">
    <label>
      <b>Name</b>
      <input v-model="transactionName" type="text" v-on:change="updateTransaction"/>
      <i>Description</i>
    </label>
    <label>
      <b>Amount</b>
      <input v-model="transactionAmount" type="number" v-on:change="updateTransaction"/>
      <i>AUD</i>
    </label>
  </div>
  <div class="form-row">
    <label>
      <b>Account</b>
      <select v-model="transactionAccount" v-on:change="updateTransaction">
        <option v-for="(value, key) in transactionAccounts" :key="key" :value="key">{{ value.name }}</option>
      </select>
      <i></i>
    </label>
    <label>
      <b>Date</b>
      <input v-model="transactionDate" type="date" v-on:change="updateTransaction"/>
      <i></i>
    </label>
  </div>
  <div class="form-row">
    <label>
      <b>Recurring</b>
      <select v-model="transactionInterval" v-on:change="updateTransaction">
        <option value="once">Once</option>
        <option value="yearly">Yearly</option>
        <option value="biannually">Biannually</option>
        <option value="quarterly">Quarterly</option>
        <option value="monthly">Montly</option>
        <option value="fortnightly">Fortnightly</option>
        <option value="weekly">Weekly</option>
        <option value="daily">Daily</option>
      </select>
      <i></i>
    </label>
  </div>
</section>
</template>

<script>
export default {
  name: 'TransactionEdit',
  props: ['id', 'transaction', 'accounts'],
  watch: {
    id () {
      this.transactionId = this.id
      this.transactionName = this.transaction.name
      this.transactionAmount = this.transaction.amount
      this.transactionAccount = this.transaction.account
      this.transactionDate = this.transaction.date
      this.transactionInterval = this.transaction.interval
    }
  },
  methods: {
    updateTransaction (evt) {
      // emit the new values
      this.$emit('update-transaction', this.transactionId, {
        name: this.transactionName,
        amount: parseFloat(this.transactionAmount),
        account: this.transactionAccount,
        date: this.transactionDate,
        interval: this.transactionInterval
      })
    }
  },
  data () {
    return {
      transactionId: this.id,
      transactionName: this.transaction.name,
      transactionAmount: this.transaction.amount,
      transactionAccount: this.transaction.account,
      transactionAccounts: this.accounts,
      transactionDate: this.transaction.date,
      transactionInterval: this.transaction.interval
    }
  }
}
</script>

<style>
.transaction-edit {
  position: absolute;
  left: 0;
  bottom: 4rem;
  right: 0;
  top: 50%;
  background-color: #fff;
  box-shadow: 0 -0.25rem 0.5rem rgba(0, 0, 0, 0.1);
  z-index: 3;
  transition: transform ease 500ms;
  transform: translate(0%, 150%);
  padding: 0.667rem 2rem;
  overflow-y: auto;
  -webkit-overflow-scrolling: touch;
}
.navigation-transaction .transaction-edit {
  transform: translate(0%, 0%);
}
</style>

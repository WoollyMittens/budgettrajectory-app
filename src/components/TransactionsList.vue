<template>
<aside class="transactions-list">
  <button class="transactions-add" v-on:click="$emit('add-transaction')">Add</button>
  <ul>
    <transaction-item
      v-for="(transaction, index) in transactions"
      :key="'transaction-' + index"
      :index="index"
      :transaction="transaction"
      :colour="accounts[transaction.account].colour"
      v-on:edit-transaction="editTransaction"/>
  </ul>
</aside>
</template>

<script>
import TransactionItem from '../components/TransactionItem'

export default {
  name: 'TransactionsList',
  props: ['transactions', 'accounts'],
  components: {
    TransactionItem
  },
  methods: {
    editTransaction (index) {
      this.$emit('edit-transaction', index)
    }
  },
  data () {
    return {

    }
  }
}
</script>

<style>
  .transactions-list {
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
  }
  .navigation-transactions .transactions-list {
    transform: translate(0%, 0%);
  }
  .transactions-add {
    position: absolute;
    width: 5rem;
    height: 5rem;
    border-radius: 50%;
    background-color: Green;
    color: White;
    left: 50%;
    top: 0;
    transform: translate(-50%, -50%);
    box-shadow: 0 0.25rem 0.5rem rgba(0, 0, 0, 0.1);
    z-index: 1;
  }
  .transactions-list > ul {
    display: block;
    height: 100%;
    list-style: none;
    padding: 0;
    margin: 0;
    overflow-x: hidden;
    overflow-y: auto;
    -webkit-overflow-scrolling: touch;
  }
  .transactions-list > ul > li {
    border-bottom: solid 1px #ccc;
    padding: 1rem 1.5rem;
  }
</style>

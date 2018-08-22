<template>
<aside class="accounts-list">
  <ul>
    <account-item v-for="(value, key, index) in accounts" :key="'account-' + key + index" :name="key" :account="value" v-on:edit-account="editAccount"/>
    <li class="account-add"><button>Add</button></li>
  </ul>
</aside>
</template>

<script>
import AccountItem from '../components/AccountItem'

export default {
  name: 'AccountsList',
  props: ['accounts'],
  components: {
    AccountItem
  },
  methods: {
    editAccount (name) {
      this.$emit('edit-account', name)
    }
  },
  data () {
    return {

    }
  }
}
</script>

<style>
  .accounts-list {
    position: absolute;
    left: 0;
    bottom: 4rem;
    right: 0;
    top: 50%;
    background-color: #fff;
    box-shadow: 0 -0.25rem 0.5rem rgba(0, 0, 0, 0.1);
    z-index: 3;
  }
  .accounts-list > ul {
    display: block;
    height: 100%;
    list-style: none;
    padding: 0;
    margin: 0;
    overflow-x: hidden;
    overflow-y: auto;
    -webkit-overflow-scrolling: touch;
  }
  .accounts-list > ul > li {
    border-bottom: solid 1px #ccc;
    padding: 1rem 1.5rem;
  }
  .account-add {
    text-align: center;
  }
</style>

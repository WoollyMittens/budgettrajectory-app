<template>
<aside class="accounts-list">
  <button class="account-add" v-on:click="$emit('add-account')">Add</button>
  <ul>
    <account-item
      v-for="(account, key, index) in accounts"
      :key="'account-' + index + '-' + key"
      :name="key"
      :account="account"
      v-on:edit-account="editAccount"
      v-on:remove-account="removeAccount"/>
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
    },
    removeAccount (name) {
      this.$emit('remove-account', name)
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
    transition: transform ease 500ms;
    transform: translate(0%, 150%);
  }
  .navigation-accounts .accounts-list {
    transform: translate(0%, 0%);
  }
  .account-add {
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
</style>

<template>
<footer class="nav-bar">
  <nav>
    <button v-on:click="$emit('pick-navigation', 'accounts')" class="nav-accounts">Accounts</button>
    <button v-on:click="$emit('pick-navigation', 'transactions')" class="nav-transactions">Transactions</button>
    <button v-on:click="$emit('pick-navigation', 'settings')" class="nav-settings">Settings</button>
  </nav>
</footer>
</template>

<script>
export default {
  name: 'NavBar',
  data () {
    return {

    }
  }
}
</script>

<style>
  .nav-bar {
    position: absolute;
    left: 0;
    bottom: 0;
    right: 0;
    height: 4rem;
    background-color: #fff;
    box-shadow: 0 -0.25rem 0.5rem rgba(0, 0, 0, 0.1);
    z-index: 4;
  }
  .nav-bar > nav {
    display: flex;
    align-items: center;
    justify-content: space-around;
    height: 100%;
  }
  .nav-bar > nav > button {
    flex: 0;
  }
  .navigation-accounts .nav-accounts,
  .navigation-transactions .nav-transactions,
  .navigation-settings .nav-settings {
    font-weight: bold;
  }
</style>

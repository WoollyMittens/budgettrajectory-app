<template>
<footer class="nav-bar">
  <nav>
    <button>Accounts</button>
    <button>Transactions</button>
    <button>Settings</button>
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
</style>

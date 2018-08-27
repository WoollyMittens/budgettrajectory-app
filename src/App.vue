<template>
<section id="app" :class="'navigation-' + navigation">
  <title-bar/>
  <bar-chart
    :accounts="accounts"
    :transactions="transactions"
    :interval="interval"
    :duration="duration"
    :updated="new Date(updated)"/>
  <accounts-list
    :accounts="accounts"
    v-on:add-account="addAccount"
    v-on:edit-account="editAccount"
    v-on:remove-account="removeAccount"/>
  <account-edit
    v-if="account"
    :id="account"
    :account="accounts[account]"
    v-on:update-account="updateAccount"/>
  <transactions-list
    :transactions="transactions"
    :accounts="accounts"
    v-on:add-transaction="addTransaction"
    v-on:edit-transaction="editTransaction"
    v-on:remove-transaction="removeTransaction"/>
  <transaction-edit
    v-if="transaction!=null"
    :id="transaction"
    :transaction="transactions[transaction]"
    :accounts="accounts"
    v-on:update-transaction="updateTransaction"/>
  <settings-edit
    :duration="duration"
    :interval="interval"
    v-on:update-settings="updateSettings"/>
  <nav-bar
    :navigation="navigation"
    v-on:pick-navigation="pickNavigation"/>
</section>
</template>

<script>
import TitleBar from './components/TitleBar'
import BarChart from './components/BarChart'
import AccountsList from './components/AccountsList'
import AccountEdit from './components/AccountEdit'
import TransactionsList from './components/TransactionsList'
import TransactionEdit from './components/TransactionEdit'
import SettingsEdit from './components/SettingsEdit'
import NavBar from './components/NavBar'

export default {
  name: 'App',
  components: {
    TitleBar,
    BarChart,
    AccountsList,
    AccountEdit,
    TransactionsList,
    TransactionEdit,
    SettingsEdit,
    NavBar
  },
  methods: {
    addAccount () {
      var index = Object.keys(this.accounts).length
      // create a new account
      this.account = 'account-' + index
      this.accounts[this.account] = {
        'name': 'Account ' + index,
        'funds': 0,
        'colour': 'green',
        'credit': 0,
        'debit': 0
      }
      // cause a component update
      this.accounts = Object.assign({}, this.accounts)
      // show the editor
      this.navigation = 'account'
    },
    editAccount (name) {
      console.log('edit account', name)
      // edit an existing account
      this.account = name
      // show the editor
      this.navigation = 'account'
    },
    updateAccount (name, changes) {
      // transfer all changed values
      for (var key in changes) {
        this.accounts[name][key] = changes[key]
      }
      // cause a component update
      this.accounts = Object.assign({}, this.accounts)
    },
    removeAccount (name) {
      console.log('remove account', name)
    },
    addTransaction () {
      console.log('add transaction')
      // create a new transaction
      this.transactions.push({
        'name': '',
        'amount': 0,
        'account': Object.keys(this.accounts)[0],
        'date': new Date(),
        'interval': 'once'
      })
      this.transaction = this.transactions.length - 1
      // cause a component update
      this.transactions = Object.assign({}, this.transactions)
      // show the editor
      this.navigation = 'transaction'
    },
    editTransaction (id) {
      console.log('edit transaction', id)
      // edit an existing account
      this.transaction = id
      // show the editor
      this.navigation = 'transaction'
    },
    updateTransaction (id, changes) {
      console.log('update transaction', id, changes)
      // transfer all changed values
      for (var key in changes) {
        this.transactions[id][key] = changes[key]
      }
      // cause a component update
      this.transactions = Object.assign({}, this.transactions)
    },
    removeTransaction (id) {
      console.log('remove transaction', id)
    },
    pickNavigation (name) {
      console.log('pick navigation', name)
      // update the navigation class
      this.navigation = name
    },
    updateSettings (changes) {
      console.log('updateSettings', changes)
      for (var key in changes) {
        this[key] = changes[key]
      }
    }
  },
  data () {
    return {
      navigation: 'accounts',
      interval: 'monthly',
      duration: 3,
      updated: 'Aug 13 2018 09:00:00 GMT+1000',
      account: null,
      accounts: {
        'savings-account': {
          'name': 'Savings',
          'funds': 4000.00,
          'colour': 'blue',
          'credit': 2,
          'debit': 0
        },
        'checking-account': {
          'name': 'Checking',
          'funds': 1000.00,
          'colour': 'green',
          'credit': 1,
          'debit': 2
        },
        'credit-card': {
          'name': 'Credit',
          'funds': -600,
          'colour': 'orange',
          'credit': 0,
          'debit': 15
        }
      },
      transaction: null,
      transactions: [
        {
          'name': 'Power',
          'amount': -200.00,
          'account': 'savings-account',
          'date': 'Mon Jan 2 2017 08:00:00 GMT+1000',
          'interval': 'quarterly'
        },
        {
          'name': 'Internet',
          'amount': -60.00,
          'account': 'checking-account',
          'date': 'Tue Aug 14 2018 15:24:06 GMT+1000',
          'interval': 'monthly'
        },
        {
          'name': 'Banana',
          'amount': -1.00,
          'account': 'checking-account',
          'date': 'Wed Aug 15 2018 12:00:00 GMT+1000',
          'interval': 'once'
        }
      ]
    }
  }
}
</script>

<style>
*:focus {
    outline: none;
}
*,
*:before,
*:after {
  box-sizing: border-box;
}
html {
  font-size: 12px;
  font-family: sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
body {
  color: #333;
  overflow: hidden;
  font-size: 1.167rem;
  margin: 0;
  overflow: hidden;
}
h1 {
  font-size: 2rem;
  margin: 0 0 2rem 0;
  font-weight: normal;
}
h2 {
  font-size: 1.667rem;
  margin: 0 0 2rem 0;
  font-weight: bold;
}
h3, legend {
  font-size: 1.333rem;
  margin: 0 0 2rem 0;
  font-weight: normal;
}
p {
  margin: 0 0 2rem 0;
}
strong, b {
  font-weight: bold;
}
a {
  color: #1976D2;
}
input,
button {
  font-family: sans-serif;
}
button {
  cursor: pointer;
}
button[name=icon] {
  border: none;
  background-color: Transparent;
  background-repeat: no-repeat;
  background-position: center;
  background-size: contain;
  width: 3rem;
  height: 3rem;
  margin: 0.5rem;
  padding: 0;
  border: none;
  overflow: hidden;
  white-space: nowrap;
  text-indent: 4rem;
}
label:hover b {
  color: #1976D2;
}
input[type=text],
input[type=number],
input[type=date],
input[type=time],
textarea,
select {
  border-radius: 0;
  border: none;
  border-bottom: solid 1px #CCCCCC;
  background-color: Transparent;
  background-repeat: no-repeat;
  background-position: right center;
  background-size: auto 70%;
  margin: 0;
  padding: 0;
  height: 2.333rem;
  font-size: 1.167rem;
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
}
input[type=range] {
  margin: 0.5rem 0 0 0;
}
input[type=text]:focus,
input[type=number]:focus,
input[type=date]:focus,
input[type=time]:focus,
textarea:focus,
select:focus {
  border-bottom: solid 2px #1976D2;
}
input[type=date]:focus,
input[type=time]:focus,
select:focus {
  transform: translateY(1px);
}
textarea {
  height: 6rem;
}
select {
  background-image: url("./assets/button_backward_2.svg");
}
button[disabled] {
  cursor: default;
  opacity: 0.5;
}
button[name=cancel],
button[name=submit] {
  height: 3rem;
  background-color: #1976D2;
  box-shadow : 1px 1px 1px rgba(0,0,0,0.5);
  border: none;
  padding: 0 1.333rem;
  margin: 0.667rem 0 0;
  font-size: 1.167rem;
  text-transform: uppercase;
  color: #fff;
}
button[name=cancel] {
  background-color: #999;
}
button[name=remove],
button[name=add] {
  border: none;
  background-color: Transparent;
  padding: 0;
  background-repeat: no-repeat;
  background-position: center;
  background-size: contain;
  width: 1.5rem;
  height: 1.5rem;
  overflow: hidden;
  white-space: nowrap;
  text-indent: 3rem;
}
button[name=remove] {
  background-image: url("./assets/button_delete_2.svg");
}
button[name=add] {
  background-image: url("./assets/button_new_2.svg");
  width: 3rem;
  height: 3rem;
}
.form-row {
  border: none;
  padding: 0;
  display: flex;
  min-height: 7rem;
  margin-left: -0.5rem;
  margin-right: -0.5rem;
}
.form-row label {
  display: block;
  position: relative;
  flex: 1 1 auto;
  padding: 1.333rem 0.5rem 0;
}
.form-row label b {
  display: block;
  font-weight: normal;
  font-size: 0.857em;
}
.form-row label textarea,
.form-row label select,
.form-row label input {
  font-size: 1rem;
  width: 100%;
}
.form-row label select {
  min-width: 10.5rem;
}
.form-row label i {
  display: block;
  font-size: 0.857em;
  padding: 0.667rem 0 0;
  font-style: normal;
  color: #999;
}
.form-buttons {
  display: flex;
  flex-wrap: wrap;
  padding: 1.333rem 0 0;
  justify-content: space-between;
  min-height: 6.5rem;
}
.form-buttons b {
  flex: 1 1 auto;
  width: 100%;
  font-weight: normal;
  font-size: 0.857em;
}
.form-buttons div {
  flex: 1 1 auto;
  max-width: 48%;
}
.form-buttons div button {
  width: 100%;
}
@keyframes bounce {
  0% {
    transform: translate(-50%, -125%);
  }
  100% {
    transform: translate(-50%, -110%);
  }
}
@keyframes throb {
  0% {
    transform: scale(1.1, 1.1);
  }
  100% {
    transform: scale(1, 1);
  }
}
#app {
  position: relative;
  height: 100vh;
  width: 100%;
  font-size: 1.167rem;
  overflow: hidden;
}
</style>

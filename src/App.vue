<template>
<section id="app" :class="'navigation-' + navigation">
  <title-bar/>
  <bar-chart
    :accounts="accounts"
    :transactions="transactions"
    :interval="interval"
    :duration="duration"
    :updated="new Date(updated)"
    v-on:show-snapshot="showSnapshot"/>
  <accounts-list
    :accounts="accounts"
    v-on:add-account="addAccount"
    v-on:edit-account="editAccount"
    v-on:remove-account="removeAccount"/>
  <account-edit
    v-if="account"
    :id="account"
    :account="accounts[account]"
    v-on:update-account="updateAccount"
    v-on:cancel-account="cancelAccount"
    v-on:remove-account="removeAccount"/>
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
    v-on:update-transaction="updateTransaction"
    v-on:cancel-transaction="cancelTransaction"
    v-on:remove-transaction="removeTransaction"/>
  <settings-edit
    :duration="duration"
    :interval="interval"
    v-on:update-settings="updateSettings"/>
  <transition name="modal">
    <snapshot-modal
      v-if="snapshot"
      :snapshot="snapshot"
      :accounts="accounts"
      v-on:close-snapshot="closeSnapshot"/>
  </transition>
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
import SnapshotModal from './components/SnapshotModal'
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
    SnapshotModal,
    NavBar
  },
  methods: {

    // ACCOUNTS

    addAccount () {
      var index = Object.keys(this.accounts).length
      // create a new account
      this.account = 'account-' + index
      this.accounts[this.account] = {
        'name': 'Account ' + index,
        'funds': 0,
        'graph': true,
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
      // update the anchor date
      this.updated = new Date().toString()
      // cause a component update
      this.accounts = Object.assign({}, this.accounts)
    },
    cancelAccount () {
      // navigate to the overview
      this.navigation = 'accounts'
    },
    removeAccount (name) {
      // delete the account
      delete (this.accounts[name])
      // cause a component update
      this.accounts = Object.assign({}, this.accounts)
      // navigate to the overview
      this.navigation = 'accounts'
    },

    // TRANSACTIONS

    addTransaction () {
      // create a new transaction
      this.transactions.push({
        'name': '',
        'amount': 0,
        'account': Object.keys(this.accounts)[0],
        'date': new Date(),
        'interval': 'once'
      })
      this.transaction = this.transactions.length - 1
      // show the editor
      this.navigation = 'transaction'
    },
    editTransaction (id) {
      // edit an existing account
      this.transaction = id
      // show the editor
      this.navigation = 'transaction'
    },
    updateTransaction (id, changes) {
      // transfer all changed values
      for (var key in changes) {
        this.transactions[id][key] = changes[key]
      }
      // cause the components to update
      this.transactions = this.transactions.map(elem => elem)
    },
    cancelTransaction () {
      // navigate to the overview
      this.navigation = 'transactions'
    },
    removeTransaction (id) {
      // remove the transaction
      var transactions = []
      this.transactions.map(elem => { if (elem !== this.transactions[id]) transactions.push(elem) })
      this.transactions = transactions
      // navigate to the overview
      this.navigation = 'transactions'
    },

    // NAVIGATION

    pickNavigation (name) {
      // update the navigation class
      this.navigation = name
    },
    updateSettings (changes) {
      // merge the changes into the model
      for (var key in changes) {
        this[key] = changes[key]
      }
    },
    saveState () {
      // store transient data
      window.localStorage.setItem('updated', this.updated)
      window.localStorage.setItem('accounts', JSON.stringify(this.accounts))
      window.localStorage.setItem('transactions', JSON.stringify(this.transactions))
    },
    restoreState () {
      // load the transient data
      var updated = window.localStorage.getItem('updated')
      var accounts = window.localStorage.getItem('accounts')
      var transactions = window.localStorage.getItem('transactions')
      // add it to the model
      if (updated) this.updated = updated
      if (accounts) this.accounts = JSON.parse(accounts)
      if (transactions) this.transactions = JSON.parse(transactions)
    },

    // MODALS

    showSnapshot (snapshot) {
      this.snapshot = snapshot
    },
    closeSnapshot () {
      this.snapshot = null
    }

  },
  updated () {
    // save the transient date
    this.saveState()
  },
  created () {
    // load the stored data
    this.restoreState()
  },
  data () {
    return {
      navigation: 'accounts',
      interval: 'monthly',
      duration: 3,
      updated: 'Mar 8 2019 09:00:00 GMT+1000',
      snapshot: null,
      account: null,
      accounts: {
        'term-deposit': {
          'name': 'Long term',
          'graph': false,
          'funds': 38383.41,
          'colour': 'blue',
          'credit': 2.3,
          'debit': 0
        },
        'savings-account': {
          'name': 'Savings',
          'graph': true,
          'funds': 3477.13,
          'colour': 'green',
          'credit': 0.5,
          'debit': 0
        },
        'checking-account': {
          'name': 'Checking',
          'graph': true,
          'funds': 101.19,
          'colour': 'orange',
          'credit': 0,
          'debit': 0
        },
        'credit-card': {
          'name': 'Credit',
          'graph': true,
          'funds': 0,
          'colour': 'maroon',
          'credit': 0,
          'debit': 12.99
        },
        'super-annuation': {
          'name': 'Super',
          'graph': false,
          'funds': 60960.27,
          'colour': 'teal',
          'credit': 2.3,
          'debit': 0
        },
        'mum-retainer': {
          'name': 'Mum',
          'graph': false,
          'funds': 11699.90,
          'colour': 'pink',
          'credit': 2.3,
          'debit': 0
        }
      },
      transaction: null,
      transactions: [
        {
          'name': 'Wages',
          'amount': 5423.34,
          'account': 'checking-account',
          'payee': '',
          'date': 'Aug 31 2017 08:00:00 GMT+1000',
          'interval': 'monthly'
        },
        {
          'name': 'Super',
          'amount': 2054.37,
          'account': 'super-annuation',
          'payee': '',
          'date': 'Aug 31 2017 08:00:00 GMT+1000',
          'interval': 'quarterly'
        },
        {
          'name': 'Savings',
          'amount': -4133,
          'account': 'checking-account',
          'payee': 'savings-account',
          'date': 'Aug 31 2017 08:00:00 GMT+1000',
          'interval': 'monthly'
        },
        {
          'name': 'Term deposit',
          'amount': -1895,
          'account': 'savings-account',
          'payee': 'term-deposit',
          'date': 'Aug 31 2017 08:00:00 GMT+1000',
          'interval': 'monthly'
        },
        {
          'name': 'Rent',
          'amount': -956,
          'account': 'savings-account',
          'payee': '',
          'date': 'Aug 20 2017 08:00:00 GMT+1000',
          'interval': 'monthly'
        },
        {
          'name': 'Internet',
          'amount': -85,
          'account': 'savings-account',
          'payee': '',
          'date': 'Aug 6 2017 08:00:00 GMT+1000',
          'interval': 'monthly'
        },
        {
          'name': 'Telephone',
          'amount': -30,
          'account': 'savings-account',
          'payee': '',
          'date': 'Aug 6 2017 08:00:00 GMT+1000',
          'interval': 'monthly'
        },
        {
          'name': 'Patreon',
          'amount': -40,
          'account': 'savings-account',
          'payee': '',
          'date': 'Mar 4 2017 08:00:00 GMT+1000',
          'interval': 'monthly'
        },
        {
          'name': 'Petrol',
          'amount': -50,
          'account': 'checking-account',
          'payee': '',
          'date': 'Aug 15 2017 08:00:00 GMT+1000',
          'interval': 'monthly'
        },
        {
          'name': 'Power',
          'amount': -250,
          'account': 'savings-account',
          'payee': '',
          'date': 'Aug 10 2017 08:00:00 GMT+1000',
          'interval': 'quarterly'
        },
        {
          'name': 'Public transport',
          'amount': -50,
          'account': 'checking-account',
          'payee': '',
          'date': 'Aug 1 2017 08:00:00 GMT+1000',
          'interval': 'weekly'
        },
        {
          'name': 'Groceries',
          'amount': -25,
          'account': 'checking-account',
          'payee': '',
          'date': 'Aug 1 2017 08:00:00 GMT+1000',
          'interval': 'weekly'
        },
        {
          'name': 'Meals',
          'amount': -30,
          'account': 'checking-account',
          'payee': '',
          'date': 'Aug 1 2017 08:00:00 GMT+1000',
          'interval': 'daily'
        },
        {
          'name': 'Clothes',
          'amount': -500,
          'account': 'savings-account',
          'payee': '',
          'date': 'Aug 20 2017 08:00:00 GMT+1000',
          'interval': 'quarterly'
        },
        {
          'name': 'Dentistry',
          'amount': -300,
          'account': 'savings-account',
          'payee': '',
          'date': 'Aug 20 2017 08:00:00 GMT+1000',
          'interval': 'biannually'
        },
        {
          'name': 'Medical bills',
          'amount': -200,
          'account': 'savings-account',
          'payee': '',
          'date': 'Aug 1 2017 08:00:00 GMT+1000',
          'interval': 'yearly'
        },
        {
          'name': 'Memberships',
          'amount': -250,
          'account': 'savings-account',
          'payee': '',
          'date': 'Aug 1 2017 08:00:00 GMT+1000',
          'interval': 'yearly'
        },
        {
          'name': 'Car insurance',
          'amount': -400,
          'account': 'savings-account',
          'payee': '',
          'date': 'Dec 20 2017 08:00:00 GMT+1000',
          'interval': 'yearly'
        },
        {
          'name': 'Car CTA',
          'amount': -550,
          'account': 'savings-account',
          'payee': '',
          'date': 'Jun 20 2017 08:00:00 GMT+1000',
          'interval': 'yearly'
        },
        {
          'name': 'Roadside assistance',
          'amount': -200,
          'account': 'savings-account',
          'payee': '',
          'date': 'Dec 20 2017 08:00:00 GMT+1000',
          'interval': 'yearly'
        },
        {
          'name': 'Car service',
          'amount': -1200,
          'account': 'savings-account',
          'payee': '',
          'date': 'Jun 20 2017 08:00:00 GMT+1000',
          'interval': 'yearly'
        },
        {
          'name': 'Motor vehicle tax',
          'amount': -550,
          'account': 'savings-account',
          'payee': '',
          'date': 'Jun 20 2017 08:00:00 GMT+1000',
          'interval': 'yearly'
        },
        {
          'name': 'Electronics',
          'amount': -2000,
          'account': 'savings-account',
          'payee': '',
          'date': 'Jan 1 2017 08:00:00 GMT+1000',
          'interval': 'yearly'
        },
        {
          'name': 'Travel',
          'amount': -2000,
          'account': 'savings-account',
          'payee': '',
          'date': 'Jan 1 2017 08:00:00 GMT+1000',
          'interval': 'yearly'
        },
        {
          'name': 'Mum',
          'amount': -800,
          'account': 'savings-account',
          'payee': '',
          'date': 'Aug 31 2017 08:00:00 GMT+1000',
          'interval': 'quarterly'
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
.form-buttons button {
  width: 100%;
}
.modal-enter-active,
.modal-leave-active {
  transition: transform ease 500ms;
}
.modal-enter,
.modal-leave-to {
  transform: translateY(-200%);
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

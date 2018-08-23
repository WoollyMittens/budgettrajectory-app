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
    v-on:edit-account="editAccount"
    v-on:add-account="addAccount"/>
  <transactions-list
    :transactions="transactions"
    :accounts="accounts"
    v-on:edit-transaction="editTransaction"
    v-on:add-transaction="addTransaction"/>
  <settings-form
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
import TransactionsList from './components/TransactionsList'
import SettingsForm from './components/SettingsForm'
import NavBar from './components/NavBar'

export default {
  name: 'App',
  components: {
    TitleBar,
    BarChart,
    AccountsList,
    TransactionsList,
    SettingsForm,
    NavBar
  },
  methods: {
    editAccount (name) {
      console.log('edit account', name)
      // update the values
      this.accounts[name].funds += 1000
      this.accounts[name].colour = 'purple'
      // cause a component update
      this.accounts = Object.assign({}, this.accounts)
    },
    addAccount () {
      console.log('add account')
    },
    editTransaction (index) {
      console.log('edit transaction', index)
      // update the values
      this.transactions[index].amount -= 100
      this.transactions[index].interval = 'monthly'
      // cause a component update
    },
    addTransaction () {
      console.log('add transaction')
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
      accounts: {
        'savings-account': {
          'name': 'Savings',
          'order': 0,
          'funds': 4000.00,
          'colour': 'blue',
          'interest': {
            'credit': 2,
            'debit': 0
          }
        },
        'checking-account': {
          'name': 'Checking',
          'order': 0,
          'funds': 1000.00,
          'colour': 'green',
          'interest': {
            'credit': 1,
            'debit': 2
          }
        },
        'credit-card': {
          'name': 'Credit',
          'order': 0,
          'funds': -600,
          'colour': 'orange',
          'interest': {
            'credit': 0,
            'debit': 15
          }
        }
      },
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
*, *:before, *:after {
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
#app {
  position: relative;
  height: 100vh;
  width: 100%;
  font-size: 1.167rem;
  overflow: hidden;
}
#app *:focus {
    outline: none;
}
#app *,
#app *:after,
#app *:before {
  box-sizing: border-box;
}
#app input,
#app button {
  font-family: sans-serif;
}
#app button {
  cursor: pointer;
}
#app .balancer-preset,
#app button[name=icon] {
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
#app label:hover b {
  color: #1976D2;
}
#app input[type=text],
#app input[type=number],
#app input[type=date],
#app input[type=time],
#app textarea,
#app select {
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
#app input[type=range] {
  margin: 0.5rem 0 0 0;
}
#app input[type=text]:focus,
#app input[type=number]:focus,
#app input[type=date]:focus,
#app input[type=time]:focus,
#app textarea:focus,
#app select:focus {
  border-bottom: solid 2px #1976D2;
}
#app input[type=date]:focus,
#app input[type=time]:focus,
#app select:focus {
  transform: translateY(1px);
}
#app textarea {
  height: 6rem;
}
#app select {
  background-image: url("./assets/button_backward_2.svg");
}
#app button[disabled] {
  cursor: default;
  opacity: 0.5;
}
#app button[name=reset],
#app button[name=confirm] {
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
#app button[name=remove],
#app button[name=add] {
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
#app button[name=remove] {
  background-image: url("./assets/button_delete_2.svg");
}
#app button[name=add] {
  background-image: url("./assets/button_new_2.svg");
  width: 3rem;
  height: 3rem;
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
</style>

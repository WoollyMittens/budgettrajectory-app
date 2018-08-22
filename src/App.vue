<template>
<section id="app">
  <title-bar/>
  <bar-chart
    :accounts="accounts"
    :transactions="transactions"
    :interval="interval"
    :duration="duration"
    :updated="new Date(updated)"/>
  <accounts-list
    :accounts="accounts"
    v-on:edit-account="editAccount"/>
  <nav-bar/>
</section>
</template>

<script>
import TitleBar from './components/TitleBar'
import BarChart from './components/BarChart'
import AccountsList from './components/AccountsList'
import NavBar from './components/NavBar'

export default {
  name: 'App',
  components: {
    TitleBar,
    BarChart,
    AccountsList,
    NavBar
  },
  methods: {
    editAccount (name) {
      // update the values
      this.accounts[name].funds += 1000
      this.accounts[name].colour = 'purple'
      // cause a component update
      this.accounts = Object.assign({}, this.accounts)
    }
  },
  data () {
    return {
      interval: 'monthly',
      duration: (4 * 365 * 24 * 60 * 60 * 1000),
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
          'category': 'Utilities',
          'amount': -200.00,
          'account': 'savings-account',
          'date': 'Mon Jan 2 2017 08:00:00 GMT+1000',
          'interval': 'quarterly'
        },
        {
          'name': 'Internet',
          'category': 'Utilities',
          'amount': -60.00,
          'account': 'checking-account',
          'date': 'Tue Aug 14 2018 15:24:06 GMT+1000',
          'interval': 'monthly'
        },
        {
          'name': 'Banana',
          'category': 'Meals',
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
html {
  font-family: sans-serif;
  font-size: 12px;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
body {
  color: #333;
  margin: 0;
  overflow: hidden;
  font-size: 1.167rem;
}
#app {}
*, *:before, *:after {
  box-sizing: border-box;
}
</style>

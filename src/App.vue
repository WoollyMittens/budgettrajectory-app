<template>
<div id="app">
  <title-bar/>
  <bar-chart :accounts="accounts" :transactions="transactions" :interval="interval" :updated="new Date(updated)"/>
</div>
</template>

<script>
import TitleBar from './components/TitleBar'
import BarChart from './components/BarChart'

export default {
  name: 'App',
  components: {
    TitleBar,
    BarChart
  },
  data () {
    return {
      interval: 'monthly',
      updated: 'Aug 13 2018 09:00:00 GMT+1000',
      accounts: {
        'savings-account': {
          'name': 'Savings',
          'order': 0,
          'funds': 4000.00,
          'interest': {
            'credit': 2,
            'debit': 0
          }
        },
        'checking-account': {
          'name': 'Checking',
          'order': 0,
          'funds': 1000.00,
          'interest': {
            'credit': 1,
            'debit': 2
          }
        },
        'credit-card': {
          'name': 'Credit',
          'order': 0,
          'funds': -600,
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
#app {
  font-family: sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #333;
}
*, *:before, *:after {
  box-sizing: border-box;
}
</style>

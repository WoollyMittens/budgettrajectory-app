<template>
<figure>
  <bar-chart-point
    v-for="(value, key, index) in timeline"
    :key="'timeline-' + index"
    :date="key"
    :values="value"
    :interval="interval"
    v-on:pick-date="pickDate"/>
  <figcaption>{{ interval }}</figcaption>
</figure>
</template>

<script>
import BarChartPoint from '../components/BarChartPoint'

export default {
  name: 'BarChart',
  props: ['accounts', 'transactions', 'interval', 'updated'],
  components: {
    BarChartPoint
  },
  created () {
    this.populate()
  },
  methods: {
    pickDate (date) {
      console.log('pickDate', date)
    },

    getKey (date) {
      // parse the date
      date = new Date(date)
      // add the year to the key
      var key = date.getFullYear()
      // add the month to the key
      if (/monthly|daily/.test(this.interval)) key += '-' + (date.getMonth() + 1)
      // add the day to the key
      if (/daily/.test(this.interval)) key += '-' + date.getDate()
      // format a key out of it
      return key
    },

    addInterval (date, interval) {
      // add the recurrance interval
      switch (interval) {
        case 'yearly': date.setFullYear(date.getFullYear() + 1); break
        case 'biannual': date.setMonth(date.getMonth() + 6); break
        case 'quarterly': date.setMonth(date.getMonth() + 3); break
        case 'monthly': date.setMonth(date.getMonth() + 1); break
        case 'fortnightly': date.setDate(date.getDate() + 14); break
        case 'weekly': date.setDate(date.getDate() + 7); break
        case 'daily': date.setDate(date.getDate() + 1); break
        default: date.setFullYear(date.getFullYear() + 1000)
      }
      // return it
      return date
    },

    populate () {
      // create a timeline
      this.createTimeline()
      // for all transactions
      var key
      for (key in this.transactions) {
        // apply the transaction to the timeline
        this.recurTransaction(this.transactions[key])
      }
      // for all accounts
      for (key in this.accounts) {
        // propagate the funds
        this.propagateFunds(this.accounts[key].funds, key)
      }
    },

    propagateFunds (startingFunds, accountKey) {
      var previousFunds = startingFunds
      var addedInterest = 0
      // for every point on the timeline
      for (var dateKey in this.timeline) {
        // TODO: calculate the interest since the previous iteration
        // propagate the starting funds across the transactions
        this.timeline[dateKey][accountKey] += startingFunds + addedInterest
        startingFunds = this.timeline[dateKey][accountKey]
        // remember the current iteration
        previousFunds = startingFunds
      }
    },

    recurTransaction (transaction) {
      var key
      // from the start date of the transaction
      var date = new Date(transaction.date)
      // while the end date is not reached
      while (date < this.end) {
        // generate the key
        key = this.getKey(date)
        // if the date is on the timeline
        if (this.timeline[key]) {
          // apply the transaction to the account
          this.timeline[key][transaction.account] += transaction.amount
        }
        // add the recurrance interval
        date = this.addInterval(date, transaction.interval)
      }
    },

    addAccounts () {
      // create an empty object
      var accountLevels = {}
      // fill it with a blank entry for each account
      for (var key in this.accounts) {
        accountLevels[key] = 0
      }
      // return it
      return accountLevels
    },

    createTimeline () {
      // project the timeline
      this.timeline = {}
      var date = new Date(this.updated)
      // while the end date is not reached
      while (date < this.end) {
        // create a timeline entry
        this.timeline[this.getKey(date)] = this.addAccounts()
        // add a graph interval
        date = this.addInterval(date, this.interval)
      }
    }
  },
  data () {
    return {
      end: new Date(this.updated.getTime() + (365 * 24 * 60 * 60 * 1000)),
      timeline: {
        /*
        '2018-6-13': {
          'savings-account': 1000,
          'checking-account': 600,
          'credit-card': 0
        },
        '2018-6-14': {
          'savings-account': 900,
          'checking-account': 700,
          'credit-card': 0
        },
        '2018-6-15': {
          'savings-account': 800,
          'checking-account': 800,
          'credit-card': 0
        }
        */
      }
    }
  }
}
</script>

<style scoped>

</style>

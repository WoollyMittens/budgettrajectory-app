<template>
<figure class="bar-chart">
  <div class="bar-chart-scroll">
    <bar-chart-point
      v-for="(value, key, index) in timeline"
      :key="'timeline-' + index"
      :values="value"
      :min="min"
      :max="max"
      :legend="legend"
      :interval="interval"
      v-on:pick-date="pickDate"/>
  </div>
  <figcaption>{{ interval }}</figcaption>
</figure>
</template>

<script>
import BarChartPoint from '../components/BarChartPoint'

export default {
  name: 'BarChart',
  props: ['accounts', 'transactions', 'interval', 'duration', 'updated'],
  components: {
    BarChartPoint
  },
  created () {
    this.recalc()
  },
  watch: {
    accounts () {
      this.recalc()
    },
    transactions () {
      this.recalc()
    },
    interval () {
      this.recalc()
    },
    updated () {
      this.recalc()
    }
  },
  methods: {
    recalc () {
      // create a timeline
      this.createTimeline()
      // create the legend
      this.createLegend()
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
      // find the limits
      this.findLimits()
    },
    pickDate (date, values) {
      console.log('pickDate', date, values)
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
    interestInterval (interest, interval) {
      // add the recurrance interval
      switch (interval) {
        case 'biannual': return interest / 2
        case 'quarterly': return interest / 4
        case 'monthly': return interest / 12
        case 'fortnightly': return interest / 26
        case 'weekly': return interest / 52
        case 'daily': return interest / 365
        default: return interest
      }
    },
    findLimits () {
      var min, max
      // for all timeline entries
      for (var time in this.timeline) {
        // for all accounts
        min = 0
        max = 0
        for (var account in this.accounts) {
          // add only the positive amounts
          if (this.timeline[time][account] > 0) max += this.timeline[time][account]
          // add only the negative amounts
          if (this.timeline[time][account] < 0) min += this.timeline[time][account]
        }
        // store if the largest
        this.max = Math.max(this.max, max)
        // store if the smallest
        this.min = Math.min(this.min, min)
      }
    },
    propagateFunds (startingFunds, accountKey) {
      var interest = this.interestInterval(
        (startingFunds > 0) ? this.accounts[accountKey].interest.credit : this.accounts[accountKey].interest.debit,
        this.interval
      )
      // for every point on the timeline
      for (var dateKey in this.timeline) {
        // propagate the starting funds across the transactions
        this.timeline[dateKey][accountKey] += startingFunds * (interest / 100 + 1)
        startingFunds = this.timeline[dateKey][accountKey]
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
    addAccounts (date) {
      // create an empty object
      var accountLevels = {
        'date': date
      }
      // fill it with a blank entry for each account
      for (var key in this.accounts) {
        accountLevels[key] = 0
      }
      // return it
      return accountLevels
    },
    createLegend () {
      // create a legend object
      for (var key in this.accounts) {
        this.legend[key] = this.accounts[key].colour
      }
    },
    createTimeline () {
      // project the timeline
      this.timeline = {}
      // while the end date is not reached
      var date = new Date(this.updated)
      while (date < this.end) {
        // create a timeline entry
        this.timeline[this.getKey(date)] = this.addAccounts(new Date(date))
        // add a graph interval
        date = this.addInterval(date, this.interval)
      }
    }
  },
  data () {
    return {
      end: new Date(this.updated.getTime() + this.duration),
      max: 100,
      min: -100,
      legend: {
        /*
        'savings-account': 'blue',
        'checking-account': 'green',
        'credit-card': 'orange'
        */
      },
      timeline: {
        /*
        '2018-6-13': {
          'date': 'Mon Jan 2 2017 08:00:00 GMT+1000',
          'savings-account': 1000,
          'checking-account': 600,
          'credit-card': 0
        },
        '2018-6-14': {
          'date': 'Mon Jan 2 2017 08:00:00 GMT+1000',
          'savings-account': 900,
          'checking-account': 700,
          'credit-card': 0
        },
        '2018-6-15': {
          'date': 'Mon Jan 2 2017 08:00:00 GMT+1000',
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

<style>
  .bar-chart {
    width: 100%;
    margin: 0;
    position: absolute;
    top: 4rem;
    left: 0;
    bottom: 50%;
    right: 0;
    z-index: 2;
  }
  .bar-chart-scroll {
    display: flex;
    justify-content: flex-start;
    align-items: flex-start;
    width: 100%;
    height: 100%;
    overflow-x: auto;
    -webkit-overflow-scrolling: touch;
  }
  .bar-chart figcaption {
    position: absolute;
    color: white;
    background-color: rgba(0,0,0,0.5);
    padding: 0.1rem 0.3rem;
    border-radius: 0.2rem;
    right: 1rem;
    top: 1rem;
  }
</style>

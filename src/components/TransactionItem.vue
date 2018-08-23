<template>
<li class="transaction-row" v-on:touchstart="onTouchStarted" v-on:touchmove="onTouchMoved" v-on:touchend="onTouchEnded" :style="rowStyles">
  <div class="transaction-legend" :style="legendStyles()">Red</div>
  <dl class="transaction-info" v-on:click="$emit('edit-transaction', index)">
    <dt class="transaction-title">{{ transaction.name }}</dt>
    <dd class="transaction-funds">$ {{ transaction.amount.toFixed(2) }}</dd>
  </dl>
  <dl class="transaction-info" v-on:click="$emit('edit-transaction', index)">
    <dt class="transaction-date-label">Date:</dt><dd class="transaction-date-value">{{ this.formatDate(transaction.date) }}</dd>
    <dt class="transaction-interval-label">Interval:</dt><dd class="transaction-interval-value">{{ transaction.interval }}</dd>
  </dl>
  <div class="transaction-delete-button" v-on:click="onDeleteTransaction"><button>Delete</button></div>
</li>
</template>

<script>
/*
'name': 'Power',
'amount': -200.00,
'account': 'savings-account',
'date': 'Mon Jan 2 2017 08:00:00 GMT+1000',
'interval': 'quarterly'
*/

export default {
  name: 'TransactionsItem',
  props: ['index', 'transaction', 'colour'],
  mounted () {},
  methods: {
    legendStyles () {
      return {
        backgroundColor: this.colour
      }
    },
    formatDate (date) {
      return new Date(date).toLocaleDateString('en-AU', { day: 'numeric', month: 'short', year: 'numeric' }).replace(/\./g, '')
    },
    onEditMode (evt) {
      console.log('switch to transaction edit mode')
    },
    onDeleteTransaction (evt) {
      console.log('delete the transaction')
    },
    onTouchStarted (evt) {
      if (evt.touches.length > 0) {
        clearTimeout(this.timeout)
        this.dragStart = evt.touches[0].screenX
        this.rowStyles.transition = 'none'
      }
    },
    onTouchMoved (evt) {
      if (evt.touches.length > 0 && this.dragStart) {
        this.dragPosition += evt.touches[0].screenX - this.dragStart
        this.dragPosition = Math.max(Math.min(this.dragPosition, 0), -96)
        this.rowStyles.transform = 'translateX(' + this.dragPosition + 'px)'
        this.dragStart = evt.touches[0].screenX
      }
    },
    onTouchEnded (evt) {
      if (this.dragStart) {
        this.timeout = setTimeout(() => {
          this.dragStart = null
          this.dragPosition = 0
          this.rowStyles.transition = 'transform 500ms ease'
          window.requestAnimationFrame(() => { this.rowStyles.transform = 'translateX(0)' })
        }, 1000)
      }
    }
  },
  data () {
    return {
      timeout: null,
      dragStart: null,
      dragPosition: 0,
      rowStyles: {
        transform: 'translateX(0)',
        transition: 'transform ease 500ms'
      }
    }
  }
}
</script>

<style>
  .transaction-row {
    position: relative;
  }
  .transaction-legend {
    position: absolute;
    left: 0;
    top: 0;
    bottom: 0;
    width: 0.5rem;
    background-color: Red;
    overflow: hidden;
    white-space: nowrap;
    text-indent: 110%;
  }
  .transaction-info {
    display: flex;
    flex-wrap: nowrap;
    align-items: center;
    margin: 0;
    width: 100%;
  }
  .transaction-info dt,
  .transaction-info dd {
    margin: 0.5rem;
  }
  .transaction-title {
    flex: 1;
    font-size: 1.333rem;
  }
  .transaction-funds {
    flex: 0;
    font-size: 1.333rem;
    text-align: right;
    white-space: nowrap;
  }
  .transaction-colour {
    flex: 0;
    text-align: right;
    display: flex;
    align-items: center;
  }
  .transaction-colour span {
    display: inline-block;
    overflow: hidden;
    white-space: nowrap;
    text-indent: 110%;
    width: 1.333rem;
    height: 1.333rem;
    background-color: Red;
  }
  .transaction-date-label,
  .transaction-interval-label {
    flex: 0;
    display: none;
  }
  .transaction-date-value,
  .transaction-interval-value {
    flex: 1;
    text-align: left;
  }
  .transaction-interval-value {
    text-align: right;
  }
  .transaction-edit-button button {}
  .transaction-delete-button {
    background-color: Red;
    position: absolute;
    left: 100%;
    top: 0;
    bottom: 0;
    width: 8rem;
  }
</style>

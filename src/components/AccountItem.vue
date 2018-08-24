<template>
<li class="account-row" v-on:touchstart="onTouchStarted" v-on:touchmove="onTouchMoved" v-on:touchend="onTouchEnded" :style="rowStyles">
  <div class="account-legend" :style="legendStyles()">Red</div>
  <dl class="account-info" v-on:click="$emit('edit-account', name)">
    <dt class="account-title">{{ account.name }}</dt>
    <dd class="account-funds">$ {{ account.funds.toFixed(2) }}</dd>
  </dl>
  <dl class="account-info" v-on:click="$emit('edit-account', name)">
    <dt class="account-debit-label">Debit:</dt><dd class="account-debit-interest">{{ account.debit }}%</dd>
    <dt class="account-credit-label">Credit:</dt><dd class="account-credit-interest">{{ account.credit }}%</dd>
  </dl>
  <div class="account-delete-button" v-on:click="onDeleteAccount"><button>Delete</button></div>
</li>
</template>

<script>
export default {
  name: 'AccountsItem',
  props: ['name', 'account'],
  mounted () {},
  methods: {
    legendStyles () {
      return {
        backgroundColor: this.account.colour
      }
    },
    onEditMode (evt) {
      console.log('switch to account edit mode')
    },
    onDeleteAccount (evt) {
      console.log('delete the account')
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
  .account-row {
    position: relative;
  }
  .account-legend {
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
  .account-info {
    display: flex;
    flex-wrap: nowrap;
    align-items: center;
    margin: 0;
    width: 100%;
  }
  .account-info dt,
  .account-info dd {
    margin: 0.5rem;
  }
  .account-title {
    flex: 1;
    font-size: 1.333rem;
  }
  .account-funds {
    flex: 0;
    font-size: 1.333rem;
    text-align: right;
    white-space: nowrap;
  }
  .account-colour {
    flex: 0;
    text-align: right;
    display: flex;
    align-items: center;
  }
  .account-colour span {
    display: inline-block;
    overflow: hidden;
    white-space: nowrap;
    text-indent: 110%;
    width: 1.333rem;
    height: 1.333rem;
    background-color: Red;
  }
  .account-debit-label {
    flex: 0;
  }
  .account-debit-interest {
    flex: 1;
    text-align: left;
  }
  .account-credit-label,
  .account-credit-interest {
    flex: 0;
    text-align: right;
  }
  .account-edit-button button {}
  .account-delete-button {
    background-color: Red;
    position: absolute;
    left: 100%;
    top: 0;
    bottom: 0;
    width: 8rem;
  }
</style>

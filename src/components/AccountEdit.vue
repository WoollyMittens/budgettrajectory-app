<template>
<section class="account-edit">
  <div class="form-row">
    <label>
      <b>Name</b>
      <input v-model="accountName" type="text" v-on:change="updateAccount"/>
      <i>{{ accountId }}</i>
    </label>
    <label>
      <b>Funds</b>
      <input v-model="accountFunds" type="number" v-on:change="updateAccount"/>
      <i>AUD</i>
    </label>
  </div>
  <div class="form-row">
    <label>
      <b>Credit Interest</b>
      <input v-model="accountCredit" type="text" v-on:change="updateAccount"/>
      <i>% p.a.</i>
    </label>
    <label>
      <b>Debit Interest</b>
      <input v-model="accountDebit" type="number" v-on:change="updateAccount"/>
      <i>% p.a.</i>
    </label>
  </div>
  <div class="form-row">
    <label>
      <b>Colour</b>
      <select v-model="accountColour" v-on:change="updateAccount">
        <option value="red">Red</option>
        <option value="green">Green</option>
        <option value="blue">Blue</option>
      </select>
      <i></i>
    </label>
  </div>
  <footer class="form-buttons">
    <div><button name="cancel" v-on:click="cancelAccount">Cancel</button></div>
    <div><button name="submit" v-on:click="updateAccount">Save</button></div>
  </footer>
</section>
</template>

<script>
export default {
  name: 'AccountEdit',
  props: ['id', 'account', 'existing'],
  watch: {
    id () {
      this.accountId = this.id
      this.accountName = this.account.name
      this.accountFunds = this.account.funds
      this.accountColour = this.account.colour
      this.accountCredit = this.account.credit
      this.accountDebit = this.account.debit
    }
  },
  methods: {
    updateAccount (evt) {
      // emit the new values
      this.$emit('update-account', this.accountId, {
        name: this.accountName,
        funds: this.accountFunds,
        colour: this.accountColour,
        credit: this.accountCredit,
        debit: this.accountDebit
      })
    },
    cancelAccount (evt) {
      // if this is a new entry, delete it
      // else, just navigate away
    }
  },
  data () {
    return {
      'accountId': this.id,
      'accountName': this.account.name,
      'accountFunds': this.account.funds,
      'accountColour': this.account.colour,
      'accountCredit': this.account.credit,
      'accountDebit': this.account.debit
    }
  }
}
</script>

<style>
.account-edit {
  position: absolute;
  left: 0;
  bottom: 4rem;
  right: 0;
  top: 50%;
  background-color: #fff;
  box-shadow: 0 -0.25rem 0.5rem rgba(0, 0, 0, 0.1);
  z-index: 3;
  transition: transform ease 500ms;
  transform: translate(0%, 150%);
  padding: 0.667rem 2rem;
  overflow-y: auto;
  -webkit-overflow-scrolling: touch;
}
.navigation-account .account-edit {
  transform: translate(0%, 0%);
}
</style>

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
      <b>Show in Graph</b>
      <select v-model="accountGraph" v-on:change="updateAccount">
        <option value="true">Show</option>
        <option value="false">Hide</option>
      </select>
      <i></i>
    </label>
    <label>
      <b>Colour</b>
      <select v-model="accountColour" v-on:change="updateAccount">
        <option value="red">Red</option>
        <option value="green">Green</option>
        <option value="blue">Blue</option>
        <option value="yellow">Yellow</option>
        <option value="magenta">Magenta</option>
        <option value="cyan">Cyan</option>
        <option value="orange">Orange</option>
        <option value="teal">Teal</option>
        <option value="pink">Pink</option>
      </select>
      <i></i>
    </label>
  </div>
  <footer class="form-buttons">
    <div><button name="cancel" v-on:click="$emit('cancel-account')">Back</button></div>
    <div><button name="submit" v-on:click="$emit('remove-account', id)">Remove</button></div>
  </footer>
</section>
</template>

<script>
export default {
  name: 'AccountEdit',
  props: ['id', 'account'],
  watch: {
    id () {
      this.accountId = this.id
      this.accountName = this.account.name
      this.accountFunds = this.account.funds
      this.accountGraph = this.account.graph
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
        funds: parseFloat(this.accountFunds),
        graph: (this.accountGraph === 'true'),
        colour: this.accountColour,
        credit: parseFloat(this.accountCredit),
        debit: parseFloat(this.accountDebit)
      })
    }
  },
  data () {
    return {
      'accountId': this.id,
      'accountName': this.account.name,
      'accountFunds': this.account.funds,
      'accountGraph': this.account.graph,
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

<template>
<section class="settings-form">
  <div class="settings-row">
    <label class="settings-forecast-value">
      <b>Forecast</b>
      <input v-model="forecastValue" id="settings-forecast-value" type="number" placeholder="3" v-on:change="updateSettings"/>
      <i>years</i>
    </label>
    <label class="settings-forecast-units">
      <b>In</b>
      <select v-model="forecastUnits" id="settings-forecast-units" v-on:change="updateSettings">
        <option value="daily">Daily</option>
        <option value="monthly" selected>Monthly</option>
        <option value="yearly">Yearly</option>
      </select>
      <i>steps</i>
    </label>
  </div>
</section>
</template>

<script>
export default {
  name: 'SettingsForm',
  props: ['duration', 'interval'],
  methods: {
    updateSettings (evt) {
      // emit the new values
      this.$emit('update-settings', {
        duration: this.forecastValue,
        interval: this.forecastUnits
      })
    }
  },
  data () {
    return {
      forecastValue: this.duration,
      forecastUnits: this.interval
    }
  }
}
</script>

<style>
.settings-form {
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
.navigation-settings .settings-form {
  transform: translate(0%, 0%);
}
.settings-form label {
  display: block;
  position: relative;
}
.settings-form label {
  padding: 1.333rem 0 0;
}
.settings-form label b {
  display: block;
  font-weight: normal;
  font-size: 0.857em;
}
.settings-form label textarea,
.settings-form label select,
.settings-form label input {
  font-size: 1rem;
  width: 100%;
}
.settings-form label select {
  min-width: 10.5rem;
}
.settings-form label i {
  display: block;
  font-size: 0.857em;
  padding: 0.667rem 0 0;
  font-style: normal;
  color: #999;
}
.settings-row {
  border: none;
  padding: 0;
  display: flex;
  min-height: 7rem;
  margin-left: -0.5rem;
  margin-right: -0.5rem;
}
.settings-row label {
  display: block;
  flex: 1 1 auto;
  padding: 1.333rem 0.5rem 0;
}
.settings-reset {
  display: flex;
  flex-wrap: wrap;
  padding: 1.333rem 0 0;
  justify-content: space-between;
  min-height: 6.5rem;
}
.settings-reset b {
  flex: 1 1 auto;
  width: 100%;
  font-weight: normal;
  font-size: 0.857em;
}
.settings-reset div {
  flex: 1 1 auto;
  max-width: 48%;
}
.settings-reset div button {
  width: 100%;
}
</style>

<template>
<section class="settings-edit">
  <div class="form-row">
    <label>
      <b>Forecast</b>
      <input v-model="forecastValue" type="number" placeholder="3" v-on:change="updateSettings"/>
      <i>years</i>
    </label>
    <label>
      <b>In</b>
      <select v-model="forecastUnits" v-on:change="updateSettings">
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
  name: 'SettingsEdit',
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
.settings-edit {
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
.navigation-settings .settings-edit {
  transform: translate(0%, 0%);
}
</style>

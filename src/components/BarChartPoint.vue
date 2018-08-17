<template>
<dl>
  <dt v-on:click="$emit('pick-date', date)">{{ date }}</dt>
  <dd v-for="(value, key, index) in values" :key="'bar-' + key + index">$ {{ value.toFixed(2) }}</dd>
</dl>
</template>

<script>
export default {
  name: 'BarChartPoint',
  props: ['date', 'values', 'interval']
}
</script>

<style scoped>

</style>

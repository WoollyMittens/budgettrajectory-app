<template>
<dl class="bar-chart-point" v-on:click="$emit('pick-date', date, values)">
  <dt class="bar-chart-date">{{ this.formatDate(values.date) }}</dt>
  <span class="bar-chart-pos" :style="barLimit(max)">
    <dd class="bar-chart-segment"
      v-for="(value, key, index) in values"
      v-if="key != 'date' && value > 0"
      :key="'bar-' + key + index"
      :class="barClass(index, key, value)"
      :style="barStyle(index, key, value)">
        $ {{ value.toFixed(2) }}
    </dd>
  </span>
  <span class="bar-chart-neg" :style="barLimit(min)">
    <dd class="bar-chart-segment"
      v-for="(value, key, index) in values"
      v-if="key != 'date' && value < 0"
      :key="'bar-' + key + index"
      :class="barClass(index, key, value)"
      :style="barStyle(index, key, value)">
        $ {{ value.toFixed(2) }}
    </dd>
  </span>
</dl>
</template>

<script>
export default {
  name: 'BarChartPoint',
  props: ['values', 'legend', 'min', 'max', 'interval'],
  methods: {
    formatDate (date) {
      return new Date(date).toLocaleDateString('en-AU', { day: 'numeric', month: 'short', year: 'numeric' }).replace(/\./g, '')
    },
    barClass (index, key, value) {
      return (value >= 0) ? 'bar-chart-point-credit' : 'bar-chart-point-debit'
    },
    barStyle (index, key, value) {
      var range = (value >= 0) ? this.max : this.min
      return {
        height: Math.abs(value / range * 100) + '%',
        backgroundColor: this.legend[key]
      }
    },
    barLimit (fraction) {
      var range = this.max - this.min
      return {
        height: Math.abs(fraction / range * 100) + '%'
      }
    }
  },
  data () {
    return {

    }
  }
}
</script>

<style>
.bar-chart-point {
  flex: 0 1 auto;
  margin: 0;
  min-width: 2rem;
  display: flex;
  flex-direction: column;
  align-items: stretch;
  justify-content: center;
  height: 100%;
  position: relative;
  z-index: 0;
}
.bar-chart-point:nth-child(5n) {
  z-index: 1;
}
.bar-chart-date {
  position: absolute;
  white-space: nowrap;
  display: none;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%) rotateZ(90deg);
  color: white;
  background-color: rgba(0,0,0,0.5);
  padding: 0.1rem 0.3rem;
  border-radius: 0.2rem;
}
.bar-chart-point:nth-child(5n) .bar-chart-date {
  display: block;
}
.bar-chart-pos,
.bar-chart-neg {
  display: flex;
  flex-direction: column;
  align-items: stretch;
  flex: 1 1 auto;
  height: 50%;
}
.bar-chart-pos {
  justify-content: flex-end;
}
.bar-chart-neg {
  justify-content: flex-start;
}
.bar-chart-segment {
  flex: 0 1 auto;
  margin: 0;
  border: solid 1px white;
  overflow: hidden;
  white-space: nowrap;
  text-indent: 110%;
}
</style>

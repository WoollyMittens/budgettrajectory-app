<template>
<dl class="bar-chart-point" v-on:click="$emit('pick-date', id)">
  <dt class="bar-chart-date">{{ this.formatDate(values.date) }}</dt>
  <span class="bar-chart-pos" :style="barLimit(max)">
    <dd class="bar-chart-segment"
      v-for="(value, key, index) in values"
      v-if="key != 'date' && value > 0"
      :key="'bar-' + key + index"
      :style="barStyle(index, key, value)">
        $ {{ value.toFixed(2) }}
    </dd>
  </span>
  <span class="bar-chart-neg" :style="barLimit(min)">
    <dd class="bar-chart-segment"
      v-for="(value, key, index) in values"
      v-if="key != 'date' && value < 0"
      :key="'bar-' + key + index"
      :style="barStyle(index, key, value)">
        $ {{ value.toFixed(2) }}
    </dd>
  </span>
</dl>
</template>

<script>
export default {
  name: 'BarChartPoint',
  props: ['id', 'values', 'legend', 'min', 'max', 'interval'],
  methods: {
    formatDate (date) {
      return new Date(date).toLocaleDateString('en-AU', { day: 'numeric', month: 'short', year: 'numeric' }).replace(/\./g, '')
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
  bottom: 52%;
  transform: translate(-50%, 0%) rotateZ(90deg);
  color: white;
  background-color: rgba(0,0,0,0.5);
  padding: 0.1rem 0.3rem;
  border-radius: 0.2rem;
  z-index: 2;
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
  position: relative;
}
.bar-chart-pos {
  justify-content: flex-end;
  padding-bottom: 2px;
}
.bar-chart-neg {
  justify-content: flex-start;
  padding-top: 2px;
}
.bar-chart-neg:before {
  content: '';
  display: block;
  position: absolute;
  left: 0;
  top: -1px;
  height: 2px;
  right: 0;
  background-color: #000;
  opacity: 0.55;
}
.bar-chart-segment {
  flex: 0 1 auto;
  margin: 0;
  border: solid 1px white;
  overflow: hidden;
  white-space: nowrap;
  text-indent: 110%;
  opacity: 0.7;
}
</style>

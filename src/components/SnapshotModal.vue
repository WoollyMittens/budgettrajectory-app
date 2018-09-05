<template>
<section :class="'snapshot-modal'">
  <button class="snapshot-close" v-on:click="$emit('close-snapshot')">Close</button>
  <table class="snapshot-data">
    <caption v-if="snapshot.date">{{ this.formatDate(snapshot.date) }}</caption>
    <tbody>
      <tr
        v-for="(value, name) in snapshot"
        v-if="name!=='date' && !isNaN(value)"
        :key="'snapshot-' + name">
        <th>{{ accounts[name].name }}:</th>
        <td>$ {{ value.toFixed(2) }}</td>
      </tr>
    </tbody>
  </table>
</section>
</template>

<script>
export default {
  name: 'SnapshotModal',
  props: ['snapshot', 'accounts'],
  methods: {
    formatDate (date) {
      return new Date(date).toLocaleDateString('en-AU', { day: 'numeric', month: 'short', year: 'numeric' }).replace(/\./g, '')
    }
  }
}
</script>

<style>
  .snapshot-modal {
    position: absolute;
    left: 2rem;
    right: 2rem;
    top: 6rem;
    padding: 1rem;
    background-color: rgba(0,0,0,0.7);
    border-radius: 2rem;
    box-shadow: 0 -0.25rem 0.5rem rgba(0, 0, 0, 0.1);
    z-index: 3;
    color: #fff;
  }
  .snapshot-close {
    position: absolute;
    right: 1rem;
    top: 1rem;
  }
  .snapshot-data {
    margin: 0 auto;
  }
  .snapshot-data caption {
    font-weight: bold;
    font-size: 1.5rem;
  }
  .snapshot-data tr {}
  .snapshot-data th {
    text-align: right;
  }
  .snapshot-data td {
    text-align: left;
  }
  .snapshot-close {
    color: #fff;
  }
</style>

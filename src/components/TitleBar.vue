<template>
<header class="title-bar">
  <h1>{{ title }}</h1>
</header>
</template>

<script>
export default {
  name: 'TitleBar',
  data () {
    return {
      title: 'Trajectory'
    }
  }
}
</script>

<style>
  .title-bar {
    position: absolute;
    left: 0;
    top: 0;
    right: 0;
    height: 4rem;
    background-color: #fff;
    box-shadow: 0 0.25rem 0.5rem rgba(0, 0, 0, 0.1);
    z-index: 4;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .title-bar h1 {
    font-size: 2rem;
    margin: 0;
  }
</style>

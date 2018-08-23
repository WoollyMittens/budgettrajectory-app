<template>
<section class="settings-form">
  <p>Settings</p>
</section>
</template>

<script>
export default {
  name: 'SettingsForm',
  data () {
    return {

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
}
.navigation-settings .settings-form {
  transform: translate(0%, 0%);
}
</style>

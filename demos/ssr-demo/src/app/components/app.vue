<template>
  <div id="app">
    <img src="../assets/images/logo.png"/>
    <p>Check <a href="https://github.com/xpepermint/vue-example/">vue-example</a> for more.</p>
    <checkbox></checkbox>
  </div>
</template>

<style lang="scss">
$mdc-theme-primary: #f44336;
$mdc-theme-accent: #f06292;

@import '@material/typography/mdc-typography';
@import '@material/ripple/mdc-ripple';
@import '@material/elevation/mdc-elevation';
@import '@material/list/mdc-list';
@import '@material/theme/mdc-theme';

@import '../assets/styles/scaffold.css';
</style>

<script>

import Checkbox from '@v-material/checkbox/Checkbox';

export default {
  components: { Checkbox }
};
</script>

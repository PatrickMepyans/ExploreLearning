<script>
import Vue from 'vue';
import ResponsiveIconGrid from './components/responsive-icon-grid.vue';

  export default Vue.extend({
    components: {
      ResponsiveIconGrid,
    },
  });

</script>

<template>
  <div id="app">
    <ResponsiveIconGrid :icons="icons"/>
  </div>
</template>

<style scoped>

</style>
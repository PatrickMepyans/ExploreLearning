<script>
import Vue from 'vue';
import ResponsiveIconGrid from './components/responsive-icon-grid.vue';

  export default Vue.extend({
    components: {
      ResponsiveIconGrid,
    },
    data() {
      return {
        icons: this.generateIconArray(),
      }
    },
    methods: {
      /*
       * generateIconArray
       * params: None
       * returns: Array of objects
       * 
       * Retreives the icons from the icon fold and returns them as an array of objects.
       *
      */
      generateIconArray() {
        return [
        { id: 1, src: 'src/assets/icons/gmail.png', label: 'Gmail' },
        { id: 2, src: 'src/assets/icons/google-apps.png', label: 'Google Apps' },
        { id: 3, src: 'src/assets/icons/google-chrome.png', label: 'Google Chrome' },
        { id: 4, src: 'src/assets/icons/google-cloud.png', label: 'Google Cloud' },
        { id: 5, src: 'src/assets/icons/google-drive.png', label: 'Google Drive' },
        { id: 6, src: 'src/assets/icons/google-play.png', label: 'Google Play' },
        { id: 7, src: 'src/assets/icons/microsoft-edge.png', label: 'Microsoft Edge' },
        ];
      },
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
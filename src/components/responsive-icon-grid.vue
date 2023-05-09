<script>
import Vue from 'vue';
import draggable from 'vuedraggable'

export default Vue.extend({
    name: 'responsive-icon-grid',
    components: {
      draggable,
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
    computed: {
      dragOptions() {
        return {
          delay: 1000,
          touchStartThreshold: 800,
        };
      },
    }
  });
</script>

<template>
  <draggable class="grid-container" v-model="icons" group="icon" @start="drag=true" @end="drag=false" v-bind="dragOptions">
   <div class="icon" v-for="icon in icons" :key="icon.id">
      <img :src="icon.src"/>
      <p> {{ icon.label }}</p>
    </div>
  </draggable>
</template>

<style scoped>
.grid-container {
  display: grid;
  grid-template-columns: repeat(6, 1fr);
  grid-gap: 10px;
  justify-items: center;
  text-align: center;
}

@media screen and (max-width: 999px) and (min-width: 500px) {
  .grid-container {
    grid-template-columns: repeat(4, 1fr);
  }
}

@media screen and (max-width: 499px) {
  .grid-container {
    grid-template-columns: repeat(2, 1fr);
  }
}

.icon {
    cursor: pointer;
  }
</style>

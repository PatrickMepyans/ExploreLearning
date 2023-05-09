<script>
/*
  This is an unused file that I'm going to keep in for clarity. During my initial build I created this component 
  for looping through the icons icons specifically. When I was doing clean up I noticed that I was just 
  emitting everything that the parent was passing through without much unique functionality. So I decided to move 
  the looping logic back to the parent and wrap up the rest of the work in there.
*/
  import Vue from 'vue';
  export default Vue.extend({
    name: 'mobile-app-icon',
    props: ['icon', 'hasTouchedOneSecond'],
    data() {
      return {
        touchTimer: 0,
      };
    },
    methods: {
      /*
       * onClick
       * params: none
       * returns: void
       * 
       * Emits the clickIcon parent method. Then checks to see if click(desktop) or touch(mobile) triggered this method.
       * If it's touch then it set touchTimer to true after one second.
       *
      */
      onClick() {
        this.$emit('clickIcon', this.icon);
      },
      /*
       * onDragOver
       * params: event - object
       * returns: void
       * 
       * Emits the dragOverIcon parent method.
       *
      */
      onDragOver(event) {
        this.$emit('dragOverIcon', event, this.icon);
      },
      /*
       * onDragEnd
       * params: event - object
       * returns: void
       * 
       * Emits the releaseIcon parent method.
       *
      */
      onDragEnd(event) {
        this.$emit('releaseIcon', event);
      },
      /*
       * onMouseLeaveOrUp
       * params: none
       * returns: void
       * 
       * Emits the resetClickTimerAndIcons parent method.
       *
      */
      onMouseLeaveOrUp() {
        this.$emit('resetClickTimerAndIcons')
      },
            /*
       * onMouseLeaveOrUp
       * params: none
       * returns: void
       * 
       * Emits the resetClickTimerAndIcons parent method.
       *
      */
      onMouseMove() {
        this.$emit('detectMouseMovement')
      },

    },
  });
</script>

<template>
  <!-- <mobile-app-icon
    v-for="icon in icons"
    :key="icon.id"
    :icon="icon"
    :hasTouchedOneSecond="hasTouchedOneSecond"
    @onIconClickorTouch="onIconClickorTouch"
    @releaseIcon="releaseIcon"
    @dragOverIcon="dragOverIcon"
    @resetClickTimerAndIcons="resetClickTimerAndIcons"
    @setHoveredIconUsingLabel="setHoveredIconUsingLabel"
    @detectMouseMovement="detectMouseMovement"
  /> -->
</template>

<style scoped>

</style>
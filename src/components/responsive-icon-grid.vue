<script>
import Vue from 'vue';
import MobileAppIcon from './mobile-app-icon.vue';

export default Vue.extend({
    name: 'responsive-icon-grid',
    components: {
      MobileAppIcon,
    },
    props: ['icons'],
    data() {
      return {
        draggedIcon: {
          id: null,
          src: null,
          label: null,
        },
        hoveredIcon: {
          id: null,
          src: null,
          label: null,
        },
        clickTimer: 0,
        hasCursorMoved: false,
      };
    },
    methods: {
      /*
       * onIconClickorTouch
       * params: icon - object
       * returns: void
       * 
       * This triggers when the user clicks or touches an icon with the grid column.
       * This is the start of the click/drag sequence, after a data reset it starts a
       * one second timer. If the cursor hasn't moved after a second the drag sequence 
       * will begin by setting draggedIcon.
       *
      */
      onIconClickorTouch(icon) {
        this.resetClickTimerAndIcons();
        this.hasCursorMoved = false;
        this.clickTimer = setTimeout(() => {
          if (!this.hasCursorMoved) {
            this.draggedIcon = icon;
          }
        }, 1000);
      },
      /*
       * swapIconPosition
       * params: event - object
       * returns: void
       * 
       * When a icon being dragged is hovered/placed over another icon they will swap positions in the icon array.
       * If there is not a hovered and dragged icon it will set cursorMoved to true, and also clear the clicktimer.
       *
      */
      swapIconPosition(event, icon) {
        event.preventDefault();
        this.hoveredIcon = icon;
        if (this.hoveredIcon?.id && this.draggedIcon?.id) {
          const fromIndex = this.icons.indexOf(this.draggedIcon);
          const toIndex = this.icons.indexOf(this.hoveredIcon);
          if (fromIndex !== toIndex) {
            this.icons.splice(fromIndex, 1);
            this.icons.splice(toIndex, 0, this.draggedIcon);
          }
        }
        this.hasCursorMoved = true;
        clearTimeout(this.clickTimer);
      },
      /*
       * clearIconData
       * params: none
       * returns: icon - object
       * 
       * Returns an empty icon object.
       *
      */
      clearIconData() {
        return {
          id: null,
          src: null,
          label: null,
        };
      },
      /*
       * resetClickTimerAndIcons
       * params: none
       * returns: void
       * 
       * Clears any icon data, and clears the clickTimer timeout.
       *
      */
      resetClickTimerAndIcons() {
        this.draggedIcon = this.clearIconData();
        this.hoveredIcon = this.clearIconData();
        clearTimeout(this.clickTimer);
      },
      /*
       * setHoveredIconUsingLabel
       * params: labelName - string
       * returns: object
       * 
       * Searches through the icon array using the label property. When found set the
       * hovered icon equal to it and return it.
       *
      */
      setHoveredIconUsingLabel(labelName) {
        return this.hoveredIcon = this.icons.find(obj => obj.label === labelName);
      },

      /*
       * logCursorMovement
       * params: none
       * returns: object
       * 
       * Used to determine if the cursor has moved after a click or touch event has begun.
       * If the cursor moves and the draggedIcon has not been set (it will only get set
       * one second after clicking) but a hoveredIcon has been detected it will set
       * cursorMoved to true and clear the timeout. This ensures that the user needs to 
       * click/touch and hold a second before dragging.
       *
      */
      logCursorMovement() {
        if (!this.draggedIcon?.id && this.hoveredIcon?.id) {
          this.hasCursorMoved = true;
          clearTimeout(this.clickTimer);
        }
      },
      /*
       * onTouchMove
       * params: event - object
       * returns: void
       * 
       * Here's the fun stuff.
       * This is needed due to how click, touch, and drag interact with each other. Touch has no 
       * drag support built in. Clicking can utilize mouse and drag events together to differentiate
       * between dragging onto a different icons or ending the drag early/off target.
       * 
       * Touchmove does not function like dragover, and won't record the icon beneath it when dragging an icon.
       * To compensate for this, manual tracking is required. The touch event is used to track the x, y coordinates,
       * of the cursor and retrieve the element from that DOM location Then it attempts to retrieve the text
       * from the label if it's a valid target. Once that's retrieved setHoveredIconUsingLabel is used in with 
       * the label to set the hovered icon value.
       * 
       * Cursor movement is logged as the mousemove event will not fire via touch, this is to check against 
       * the one second hold down. Then if the label is valid a swap will be made. 
       *
      */
      onTouchMove(event) {
          const targetTouchEvent = event.targetTouches[event.targetTouches.length-1];
          const domNode = document.elementFromPoint(targetTouchEvent.pageX, targetTouchEvent.pageY);
          const labelValue = domNode.closest('.icon')?.querySelector('p')?.innerText;
          const hoveredIcon = this.setHoveredIconUsingLabel(labelValue);
          this.logCursorMovement();

        if (labelValue) {
          this.swapIconPosition(event, hoveredIcon);
        }
      },
    },
  });
</script>

<template>
  <div class="grid-container">
    <div 
      v-for="icon in icons"
      :key="icon.id"
      class="icon"
      :draggable="true"
      @mousedown="onIconClickorTouch(icon)"
      @mousemove="logCursorMovement()"
      @dragover="swapIconPosition($event, icon)"
      @dragend="swapIconPosition($event, icon)"
      @touchstart="onIconClickorTouch(icon)"
      @touchmove="onTouchMove($event)"
    >
      <img :src="icon.src" :alt="icon.label"/>
      <p> {{ icon.label }}</p>
    </div>
  </div>
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

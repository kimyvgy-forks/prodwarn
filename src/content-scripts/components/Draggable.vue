<template>
  <section
    class="v-draggable"
    :style="{
      'position': 'fixed',
      'top': `${this.position.top}px`,
      'left': `${this.position.left}px`,
      'z-index': 999999,
    }"
    @mousedown="dragMouseDown"
    @click.stop
  >
    <slot />
  </section>
</template>

<script lang="ts">
import { Component, Vue, Prop } from 'vue-property-decorator';

export interface FloatPosition {
  top: number;
  left: number;
}

let startupPositionX = 0;
let startupPositionY = 0;
let startupOffsetTop = 0;
let startupOffsetLeft = 0;

@Component
export default class Draggable extends Vue {
  @Prop({ default: { top: 16, left: 32 } }) initialPosition!: FloatPosition;

  position: FloatPosition = this.initialPosition;

  isDragging: boolean = false;

  dragMouseDown(event: Event) {
    const e: any = event || window.event;
    e.preventDefault();

    if (e.which !== 1) return; // is only left mouse button

    startupPositionX = e.clientX;
    startupPositionY = e.clientY;
    // @ts-ignore this.$el.offsetTop
    startupOffsetTop = this.$el.offsetTop;
    // @ts-ignore this.$el.offsetLeft
    startupOffsetLeft = this.$el.offsetLeft;

    document.onmouseup = this.closeDragElement.bind(this);
    document.onmousemove = this.elementDrag.bind(this);
  }

  closeDragElement = () => {
    // stop moving when mouse button is released:
    document.onmouseup = null;
    document.onmousemove = null;

    this.isDragging = false;
  }

  elementDrag(event: Event) {
    const e: any = event || window.event;
    const nextPosition = this.position;

    this.isDragging = true;

    e.preventDefault();

    // calculate the new cursor position:
    const distanceX = startupPositionX - e.clientX;
    const distanceY = startupPositionY - e.clientY;

    nextPosition.top = startupOffsetTop - distanceY;
    nextPosition.left = startupOffsetLeft - distanceX;

    this.position = nextPosition;

    this.$emit('moved', nextPosition);
  }
}
</script>

<template>
  <Draggable
    ref="draggable"
    :initialPosition="position"
    @moved="moved"
  >
    <div
      :title="message"
      class="prodwarn-float-icon"
      @click="toggle"
    >
      <img
        src="https://images.viblo.asia/64x-/9cd6ae80-2ec5-4382-99b4-87e8b7ca4b1e.png"
        class="prodwarn-float-message-icon"
      />
    </div>
  </Draggable>
</template>

<script lang="ts">
import _get from 'lodash/get';
import { Component, Vue, Prop } from 'vue-property-decorator';
import Draggable, { FloatPosition } from '@/content-scripts/components/Draggable.vue';

@Component({
  components: {
    Draggable,
  },
})
export default class FloatMessage extends Vue {
  @Prop({ default: '' }) message!: string;

  position: FloatPosition = { top: 16, left: 32 };

  created() {
    const position = localStorage.getItem('prodwarn-float-icon-position');
    this.position = position ? JSON.parse(position) : null;
  }

  moved(nextPosition: FloatPosition) {
    this.position = nextPosition;

    localStorage.setItem('prodwarn-float-icon-position', JSON.stringify(nextPosition));
  }

  toggle() {
    if (_get(this.$refs, 'draggable.isDragging')) return;

    console.log('hello');
  }
}
</script>

<style lang="scss" scoped>
.prodwarn-float-icon {
  @keyframes shake {
    10%, 90% {
      transform: translate3d(-1px, 0, 0);
    }

    20%, 80% {
      transform: translate3d(2px, 0, 0);
    }

    30%, 50%, 70% {
      transform: translate3d(-4px, 0, 0);
    }

    40%, 60% {
      transform: translate3d(4px, 0, 0);
    }
  }

  cursor: pointer;
  width: 40px;
  animation: shake 0.82s ease infinite;
  img {
    filter: drop-shadow(0 0 6px rgba(0,0,0,0.3));
  }
}
</style>

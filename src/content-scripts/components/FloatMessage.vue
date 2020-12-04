<template>
  <div
    :class="{
      'prodwarn-float-message': true,
      'prodwarn-float-message--bottom': true,
    }"
  >
    <div class="prodwarn-float-message-body">
      <strong>&#9888;</strong>
      <span>{{ message }}</span>
      <button
        class="prodwarn-float-message-close"
        title="Click to close"
        @click="$emit('close')"
      >
        X
      </button>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue, Prop } from 'vue-property-decorator';
import Draggable from '@/content-scripts/components/Draggable.vue';

@Component({
  components: {
    Draggable,
  },
})
export default class FloatMessage extends Vue {
  @Prop({ default: '' }) message!: string;
}
</script>

<style lang="scss" scoped>
  .prodwarn-float-message {
    position: fixed;
    left: 0;
    width: 100%;
    z-index: 999999;
    padding: 1rem;
    background: #f82f25;
    color: #fff;
    box-shadow: 0 1px 5px 5px rgba(0, 0, 0, 0.2);
    @media screen and (max-width: 1400px) {
      padding: 0.2rem 1rem;
    }
    &-body {
      font-size: 20px;
    }
    &-close {
      border: none;
      outline: none;
      background: transparent;
      color: #fff;
      display: inline-block;
      padding-left: 1rem;
      padding-right: 1rem;
      cursor: pointer;
    }

    &--bottom {
      bottom: 0;
      left: 0;
    }

    &-top {
      top: 0;
      left: 0;
    }
  }

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

  .prodwarn-float-message {
    &-minimized {
      cursor: pointer;
      width: 40px;
      animation: shake 0.82s ease infinite;
      img {
        filter: drop-shadow(0 0 6px rgba(0,0,0,0.3));
      }
    }
  }
</style>

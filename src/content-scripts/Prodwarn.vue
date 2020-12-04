<template>
  <div id="prodwarn-root">
    <FloatIcon v-show="isMinimized" :message="warning" />
    <FloatMessage v-if="!isMinimized" :message="warning" @close="toggleFloatMessage" />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import FloatIcon from '@/content-scripts/components/FloatIcon.vue';
import FloatMessage from '@/content-scripts/components/FloatMessage.vue';

@Component({
  components: { FloatIcon, FloatMessage },
})
export default class Prodwarn extends Vue {
  isMinimized: boolean = false;

  warning: string = '!!! Be careful, this is production site. Don\'t take any actions here, please!';

  created() {
    this.isMinimized = Boolean(localStorage.getItem('prodwarn-float-message-minimized'));
  }

  toggleFloatMessage() {
    this.isMinimized = !this.isMinimized;

    if (this.isMinimized) {
      localStorage.setItem('prodwarn-float-message-minimized', 'true');
    } else {
      localStorage.removeItem('prodwarn-float-message-minimized');
    }
  }
}
</script>

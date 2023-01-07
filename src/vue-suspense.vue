
<template>
  <component :is="tag" class="vue-suspense">
    <div v-if="loadingData" class="loading-container">
      <slot name="loading">
        <loading />
      </slot>
    </div>
    <slot v-else-if="hasError" name="error" v-bind:error="error" />
    <slot v-else name="default" />
  </component>
</template>

<script lang="ts">
import Vue from 'vue';
import loading from './loading.vue';

interface VueSuspenseData {
  loadingData: Boolean;
  hasError: Boolean;
  error?: any;
}

export default Vue.extend({
  name: 'VueSuspense',
  components: { loading },
  props: {
    value: {
      type: [String, Number, Boolean, Array, Object],
      default: undefined,
    },
    promise: {
      type: Promise,
      required: true,
    },
    tag: {
      type: String,
      default: 'div'
    },
  },
  data(): VueSuspenseData {
    return {
      loadingData: false,
      hasError: false,
      error: undefined,
    };
  },
  created() {
    this.loadData();
  },
  methods: {
    async loadData() {
      this.loadingData = true;
      try {
        const data: any = await this.promise;
        this.$emit('input', data);
      } catch (err: any) {
        this.error = err;
        this.hasError = true;
        this.$emit('input', undefined);
        this.$emit('on-error', err);
      } finally {
        this.loadingData = false; 
      }
    }
  },
});
</script>

<style scoped>
.loading-container {
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>

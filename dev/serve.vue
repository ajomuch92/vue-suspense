<script lang="ts">
import Vue from 'vue';
import VueSuspense from '@/vue-suspense.vue';

export default Vue.extend({
  name: 'ServeDev',
  components: {
    VueSuspense
  }
});
</script>

<template>
  <div id="app">
    <vue-suspense />
  </div>
</template>

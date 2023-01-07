<template>
  <div id="app">
    <vue-suspense v-model="dataLoaded" :promise="loadData()" :key="key">
      <ul>
        <li v-for="number in dataLoaded" :key="number">{{ number }}</li>
      </ul>
      <template #error="{error}">
        {{ error }}
      </template>
      <!-- <template #loading>
        Loading numbers...
      </template> -->
    </vue-suspense>
    <button @click="keyFlag=!keyFlag">Reload</button>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import VueSuspense from '@/vue-suspense.vue';

export default Vue.extend({
  name: 'ServeDev',
  components: {
    VueSuspense
  },
  data: () => ({
    keyFlag: false,
    dataLoaded: [],
  }),
  computed: {
    key() {
      return this.keyFlag ? 1 : -1;
    }
  },
  methods: {
    async loadData() {
      const randomSeconds: number = Math.floor(Math.random() * 6) + 1;
      console.log(randomSeconds);
      await new Promise((resolve) => setTimeout(resolve, randomSeconds * 1000));
      if (randomSeconds >= 5) {
        return Promise.reject('Timeout error');
      }
      return Array.from(Array(10), (_, index) => index + 1);
    }
  }
});
</script>
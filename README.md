
# Vue Suspense

A package to use with use with Vuejs 2 to load data while is showing a placeholder.
Also offers an error fallback template.




## Installation

Install vue-suspense with NPM

```bash
  npm install vue-suspense
```

And use it into your main file

```javascript
import Vue from 'vue';

import VueSuspense from 'vue-suspense';

Vue.use(VueSuspense);
```
## API Reference

#### Props


| Name | Type     | Description                |
| :-------- | :------- | :------------------------- |
| `id`      | `string, number, boolean, array, object` | Used as two way binding object (v-model). |
| `promise`      | `Promise` | **Required**. A promise object or a function that returns a promise. Used to load the data into the component. |
| `tag`      | `string` | **Default: div**. The name of the tag used as wrapper for the component. |

#### Events

| Name | Description                       |
| :-------- | :-------------------------------- |
| `input`     | Emitted after load the data and passed as payload. |
| `on-error`  | Emitted after an error was caught with the error as payload. |

#### Slots

| Name | Description                       |
| :-------- | :-------------------------------- |
| `default`     | Used when data is loaded and there are not errors. |
| `loading`     | Used when data is being loading. By default, show an animated spinner icon. |
| `error`  | Used when an error happened. |



## Example

```html
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
```

See an example [here](https://github.com/ajomuch92/vue-suspense/blob/main/dev/serve.vue).


## Author

- [@ajomuch92](https://www.github.com/ajomuch92)

Feel free to open a PR or create an issue to contribute to this package.


## License

[MIT](https://github.com/ajomuch92/vue-suspense/blob/main/LICENSE)


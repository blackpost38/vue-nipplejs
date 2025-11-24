# vue-nipplejs

> Vue wrapper component for nipplejs — a virtual joystick for touch capable interfaces.

Lightweight Vue 3 component that integrates the [nipplejs](https://github.com/yoannmoinet/nipplejs) virtual joystick, providing a simple API of props and events for use in Vue applications.

## Quick start

Install into your project (this package declares `vue` as a peer dependency):

```bash
npm install vue-nipplejs
npm install vue@^3   # ensure Vue 3 is installed in the consumer project
```

If you are developing the library locally, use the dev dependencies:

```bash
npm install
npm run dev
```

## Usage

Register locally (single-file component example):

```vue
<script setup>
import { ref } from 'vue'
import VueNipple from 'vue-nipplejs'

const onMove = (evt) => {
  // evt contains nipplejs data (direction, distance, vector, etc.)
  console.log('move', evt)
}
</script>

<template>
  <div class="example">
    <VueNipple :options="{ mode: 'static' }" @move="onMove" />
  </div>
</template>

<style>
.example {
  position: relative;
  height: 300px;
}
/* container should have a positioning context for the joystick */
</style>
```

Or register globally in your app:

```js
import { createApp } from 'vue'
import App from './App.vue'
import VueNipple from 'vue-nipplejs'

const app = createApp(App)
app.component('VueNipple', VueNipple)
app.mount('#app')
```

## Notes

- This package declares `vue` as a `peerDependency` — consumers must provide Vue 3 in their projects to avoid duplicate Vue instances.
- `nipplejs` is included as a runtime dependency, so it does not need to be separately installed by consumers.

## Contributing

PRs welcome. Please follow existing coding style and run formatting and lint scripts before submitting. Run tests with `npm run test:unit`.

## License

MIT — see `LICENSE` (or add a license file if needed).

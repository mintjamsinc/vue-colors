<img src="https://www.mintjams.jp/img/cr.svg" alt ="" width="64">

# vue-colors
A reusable Colors component for Vue.js 2.x used by webtop applications.

## Installation

```sh
npm install --save-dev @mintjamsinc/vue-colors
```

## Usage

```vue
<Colors
  ref="colors"
  :multiple="true"
  v-hook="{inserted: onColorsLoad}"/>
```

```js
import Colors from '@mintjamsinc/vue-colors';

export default {
  components: {
    Colors,
  },
  methods: {
    onColorsLoad() {
      // let ui = vm.$refs.colors.ui;
      // ui.onChanged = function() {};
      // ui.colors = ...
      // ui.selection = ...
    },
  },
}
```

## License

[MIT](https://opensource.org/licenses/MIT)

Copyright (c) 2021 MintJams Inc.
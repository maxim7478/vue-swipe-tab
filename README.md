# 📱 SwipeTabs

A Vue 3 component for swiping between tabs with custom content support, smooth animations, and flexible tab positioning.

[🌐 Demo](http://vue-swipe-tab.mb-works.fun/)

## ✨ Features

- 🔄 Swipe and drag to switch tabs
- 🧩 Fully typed scoped slots
- 📍 Flexible tab positions: `top`, `bottom`, `left`, `right`
- 🎨 Easily customizable styles
- 🧱 Universal — works with any data and components

## 🚀 Installation

```bash
npm install @your-namespace/swipe-tabs
# or
yarn add @your-namespace/swipe-tabs
```

## ⚙️ Props

| Prop           | Type              | Required | Description                                                             |
|----------------|-------------------|----------|-------------------------------------------------------------------------|
| `tabs`         | `Array<unknown>`  | ✅       | Array of tab items                                                      |
| `contentList`  | `Array<unknown>`  | ✅       | Array of content matching each tab (e.g., Vue components)              |
| `tabsPosition` | `"top" \| "bottom" \| "left" \| "right"` | ❌ | Tab position. Defaults to `"top"`                                      |

> ⚠️ The `tabs` and `contentList` arrays must have the same length.

## 🧩 Slots

Available slots and their typings:

### `tabs`

Customize how each tab is rendered.

| Prop       | Type       | Description                |
|------------|------------|----------------------------|
| `tab`      | `unknown`  | Data for the current tab   |
| `index`    | `number`   | Tab index                  |
| `isActive` | `boolean`  | Whether the tab is active  |

### `panels`

Customize how the tab content is rendered.

| Prop       | Type       | Description                    |
|------------|------------|--------------------------------|
| `content`  | `unknown`  | Data or component to render    |

## 🎨 Styling

You can customize the appearance of the component using your own CSS.  
A base example of the default styles can be found here:

```
vue-swipe-tabs/src/assets/theme/index.css
```

You can use it as a starting point to override or extend the component’s styles.

## 🛠 Usage Example

```vue
<template>
  <SwipeTabs
    :tabs="refTabs"
    :content-list="components"
    :tabs-position="refActivePosition"
  >
    <template #tabs="{ tab, index, isActive }">
      <button :class="{ active: isActive }">{{ index + 1 }}. {{ tab }}</button>
    </template>

    <template #panels="{ content }">
      <component :is="content" />
    </template>
  </SwipeTabs>
</template>

<script setup>
  import { ref } from 'vue'
  import SwipeTabs from 'vue-swipe-tabs'
  import TabOne from './TabOne.vue'
  import TabTwo from './TabTwo.vue'

  const refTabs = ['First', 'Second']
  const components = [TabOne, TabTwo]
  const refActivePosition = ref('top')
</script>
```

## 📄 License

[MIT](LICENSE)
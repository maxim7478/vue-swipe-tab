<template>
  <main>
    <div>
      <h1>Swipe tab view</h1>
    </div>
    <swipe-tabs :tabs="refTabs" :content-list="components">
      <template #tabs="{ tab }">
        {{ tab }}
      </template>

      <template #panels="{ content }">
        <div>{{ content }}</div>
      </template>
    </swipe-tabs>
  </main>
</template>

<script setup lang="ts">
import { SwipeTabs } from '@/components/SwipeTabs'

const refTabs = ['first', 'second', 'third']
const components = [1, 2, 3]
</script>

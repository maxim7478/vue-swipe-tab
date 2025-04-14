<template>
  <main>
    <div class="home-page">
      <h1 class="home-page__title">Vue Swipe Tab</h1>
      <button
        v-for="item in Position"
        class="home-page__button"
        :class="{ active: item === refActivePosition }"
        :key="`position-button-${item}`"
        @click="changePosition(item)"
      >
        {{ item }}
      </button>
      <swipe-tabs :tabs="refTabs" :content-list="components" :tabs-position="refActivePosition">
        <template #tabs="{ tab }">
          {{ tab }}
        </template>

        <template #panels="{ content }">
          <component :is="content" />
        </template>
      </swipe-tabs>
    </div>
  </main>
</template>

<script setup lang="ts">
import { Position, SwipeTabs } from '@/components/SwipeTabs'
import FirstTab from '@/components/example/FirstTab.vue'
import SecondTab from '@/components/example/SecondTab.vue'
import ThirdTab from '@/components/example/ThirdTab.vue'
import { ref } from 'vue'

const refActivePosition = ref<Position>(Position.Left)
const refTabs = ['first', 'second', 'third']
const components = [FirstTab, SecondTab, ThirdTab]

function changePosition(item: Position) {
  refActivePosition.value = item
}
</script>
<style scoped>
.home-page {
  margin: 0 auto;
}
.home-page__title {
  text-align: center;
}
.home-page__button {
  padding: 4px;
  background: transparent;
  color: white;
  border: 1px solid #fff;
  border-radius: 4px;
  outline: none;
  cursor: pointer;
  min-width: 60px;
  &:active {
    scale: 0.98;
  }
}
.active {
  background: white;
  color: black;
  border: 1px solid black;
}
</style>

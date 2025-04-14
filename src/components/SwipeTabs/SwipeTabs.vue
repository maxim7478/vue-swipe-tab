<template>
  <div class="swipe-tabs" :class="layoutClass">
    <div class="swipe-tabs__tabs" :class="tabsPositionClass">
      <div
        class="swipe-tabs__tabs-item"
        v-for="(item, index) in props.tabs"
        :key="`swipe-tab-index-${index}`"
        @click="setActive(index)"
        :class="{ 'swipe-tabs__tabs-active': refActiveIndex === index }"
      >
        <slot name="tabs" :tab="item" :is-active="refActiveIndex === index" :index="index" />
      </div>
    </div>
    <div class="swipe-tabs__content" ref="refSwipeArea">
      <div
        class="swipe-tabs__content-inner"
        :style="transformStyle"
        @transitionend="onTransitionEnd"
      >
        <div
          v-for="(item, index) in props.contentList"
          :key="`content-${index}`"
          class="swipe-tabs__content-item"
        >
          <slot name="panels" :content="item" />
        </div>
      </div>
    </div>
  </div>
</template>
<script setup lang="ts">
import { computed, onMounted, onUnmounted, ref } from 'vue'
import { Position } from '@/components/SwipeTabs/enums'
import type { SwipeTabsProps } from '@/components/SwipeTabs/types.ts'

const props = withDefaults(
  defineProps<SwipeTabsProps>(),
  {
    tabsPosition: Position.Top,
  },
)

defineSlots<{
  tabs(props: { tab: unknown; index: number; isActive: boolean }): never
  panels(props: { content: unknown }): never
}>()

const EDGE_THRESHOLD = 50

const refSwipeArea = ref<HTMLElement>()
const shouldSwipeFromEdge = ref(false)
const refActiveIndex = ref(0)
const touchStartX = ref(0)
const deltaX = ref(0)
const isDragging = ref(false)
const isTransitioning = ref(false)

onMounted(() => {
  if (!refSwipeArea.value) return
  refSwipeArea.value.addEventListener('touchstart', onTouchStart)
  refSwipeArea.value.addEventListener('touchmove', onTouchMove)
  refSwipeArea.value.addEventListener('touchend', onTouchEnd)
})

onUnmounted(() => {
  if (!refSwipeArea.value) return
  refSwipeArea.value.removeEventListener('touchstart', onTouchStart)
  refSwipeArea.value.removeEventListener('touchmove', onTouchMove)
  refSwipeArea.value.removeEventListener('touchend', onTouchEnd)
})

const computedTabsCount = computed(() => props.tabs.length ?? 0)
const layoutClass = computed(() => `swipe-tabs--${props.tabsPosition}`)
const tabsPositionClass = computed(() => `tabs--${props.tabsPosition}`)

function setActive(index: number) {
  if (index >= 0 && index < computedTabsCount.value) {
    refActiveIndex.value = index
  }
}

const transformStyle = computed(() => {
  const offset =
    isDragging.value && refSwipeArea.value
      ? (deltaX.value / refSwipeArea.value.offsetWidth) * 100
      : 0
  const translateX = -refActiveIndex.value * 100 + offset
  return {
    transform: `translateX(${translateX}%)`,
    transition: isDragging.value ? 'none' : 'transform 0.3s ease',
  }
})

function onTouchStart(event: TouchEvent) {
  const x = event.touches[0].clientX

  const area = refSwipeArea.value
  if (!area) return

  const rect = area.getBoundingClientRect()
  const offsetX = x - rect.left

  shouldSwipeFromEdge.value = offsetX < EDGE_THRESHOLD || rect.width - offsetX < EDGE_THRESHOLD

  if (shouldSwipeFromEdge.value) {
    touchStartX.value = x
    deltaX.value = 0
    isDragging.value = true
  }
}

function onTouchMove(event: TouchEvent) {
  if (!isDragging.value) return
  const currentX = event.touches[0].clientX
  deltaX.value = currentX - touchStartX.value
  event.preventDefault()
}

function onTouchEnd() {
  if (!isDragging.value) return
  const threshold = 50

  if (deltaX.value > threshold && refActiveIndex.value > 0) {
    refActiveIndex.value--
  } else if (deltaX.value < -threshold && refActiveIndex.value < props.tabs.length - 1) {
    refActiveIndex.value++
  }

  deltaX.value = 0
  isDragging.value = false
}

function onTransitionEnd() {
  isTransitioning.value = false
}
</script>
<style scoped>
@import "@/assets/swipe-tabs.css";
</style>

<template>
  <div class="masonry" ref="component">
    <slot></slot>
  </div>
</template>

<script setup>

import { ref, toRefs, onUpdated, onBeforeUnmount, onMounted, nextTick, unref, watch } from 'vue'
import MiniMasonry from 'minimasonry'

const component = ref(null)

const props = defineProps({
    baseWidth: {
      type: Number,
      default: 255,
    },
    gutter: {
      type: Number,
      default: 10
    },
    gutterX: {
      type: Number,
      default: null
    },
    gutterY: {
      type: Number,
      default: null    
    },
    minify: {
      type: Boolean,
      default: true
    },
    surroundingGutter: {
      type: Boolean,
      default: true
    },
    ultimateGutter: {
      type: Number,
      default: 5
    },
    direction: {
      type: String,
      default: 'ltr'
    },
    wedge: {
      type: Boolean,
      default: false
    },
})

const { baseWidth, gutter, gutterX, gutterY, minify, surroundingGutter, ultimateGutter, wedge } = toRefs(props)
let masonry

function init() {
  masonry = new MiniMasonry({
    container: component.value,
    gutter: unref(gutter),
    baseWidth: unref(baseWidth), 
    gutterX: unref(gutterX), 
    gutterY: unref(gutterY), 
    minify: unref(minify), 
    surroundingGutter: unref(surroundingGutter), 
    ultimateGutter: unref(ultimateGutter), 
    wedge: unref(wedge)
  })
}

function destroy() {
  masonry.destroy()
  masonry = undefined
}

onMounted(async () => {
  await nextTick()
  init()
})

onBeforeUnmount(destroy)

onUpdated(() => masonry?.layout())

watch(props, async () => {
  destroy()
  await nextTick()
  init()
})

</script>

<style scoped>
.masonry {
  position: relative;
  width: 100%;
}
.masonry > ::v-deep(*) {
  position: absolute;
}
</style>
<template>
  <q-img :src="img_src" :ratio="img_ratio && $q.screen.gt.sm ? img_ratio : img_ratio! / 2" class="hero">
    <div class="absolute-full text-subtitle2 flex column flex-center no-wrap text-body1">
      <slot />
    </div>
  </q-img>
</template>

<script setup lang="ts">
const { img_src, img_ratio } = defineProps({
  img_src: {
    type: String,
  },
  img_ratio: {
    type: Number,
  },
})
</script>

<style lang="scss" scoped>
.hero {
  margin-top: -98px;
}
</style>
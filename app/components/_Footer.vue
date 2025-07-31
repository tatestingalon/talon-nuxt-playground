<script setup lang="ts">
const route = useRoute()

const { data: pages } = await useAsyncData('footer-' + route.path, async () => {
  const allPages = await queryCollection('content').all()
  return allPages.filter(p => p.meta.inFooter)
})
</script>

<template>
  <q-footer class="bg-grey-8 q-py-lg text-white">
    <Container>
      <q-toolbar>
        <q-toolbar-title>
          <q-avatar>
            <img src="https://cdn.quasar.dev/logo-v2/svg/logo-mono-white.svg">
          </q-avatar>
          <span class="q-ml-sm">Talon's Nuxt Playground</span>
          <div class="text-subtitle2 q-mt-md q-gutter-md">
            <p class="footer-text">This is the footer</p>
          </div>
        </q-toolbar-title>
        <q-tabs>
          <!-- <q-route-tab :to="`${getLanguageFromPath()}/`" label="Home" />
          <q-route-tab :to="`${getLanguageFromPath()}/about`" label="About" /> -->
          <q-route-tab v-for="p in pages" v-bind:key="p.id" :to="p.path">
            <div class="q-tab__label">
              <span v-if="p.meta.name">{{ p.meta.name }}</span>
              <span v-else><abbr title="This page is missing the 'name' property.">???</abbr></span>
            </div>
          </q-route-tab>
        </q-tabs>
      </q-toolbar>
    </Container>
  </q-footer>
</template>

<style lang="css" scoped>
.footer-text {
  color: hsl(0deg 0% 100% / 50%)
}
</style>
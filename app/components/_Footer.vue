<script setup lang="ts">
const route = useRoute()
// ISO 3166-1 alpha-2
const LANGUAGES = ['en', 'de', 'rs']

const { data: allPages } = await useAsyncData('footer-' + route.path, async () => {
  const allPages = await queryCollection('content').all()
  return allPages.filter(p => p.meta.inFooter)
})

const pagesUnsorted = getLanguageFromPath() === 'en'
  ? allPages.value?.filter(p => getLanguageFromPath(p.path) === 'en')
  : allPages.value?.filter(p => getLanguageFromPath(p.path) !== 'en')

const pages = pagesUnsorted?.filter(item => typeof item.meta.footerSpot === 'number')
  .sort((a, b) => (a.meta.footerSpot as number) - (b.meta.footerSpot as number))
  .concat(pagesUnsorted.filter(item => typeof item.meta.footerSpot !== 'number'))

function getLanguageFromPath(path = route.path): string {
  for (const l of LANGUAGES) {
    if (path.split('/')[1]?.includes(l))
      return l
  }
  // return LANGUAGES[0]!
  return 'en'
}
</script>

<template>
  <q-footer class="bg-grey-8 q-pt-xl q-pb-lg text-white">
    <Container>
      <q-toolbar class="align-start flex-wrap no-ellipsis">
        <q-toolbar-title>
          <q-avatar>
            <img src="https://cdn.quasar.dev/logo-v2/svg/logo-mono-white.svg">
          </q-avatar>
          <span class="q-ml-sm">Talon's Nuxt Playground</span>
          <div class="text-subtitle2 q-mt-md q-gutter-md">
            <p class="flex column">
              <a href="https://github.com/tatestingalon/talon-nuxt-playground" class="footer-text">
                https://github.com/tatestingalon/talon-nuxt-playground
              </a>
              <a href="https://oktalon-szoradi.github.io/#/" class="footer-text">
                https://oktalon-szoradi.github.io/#/
              </a>
            </p>
          </div>
        </q-toolbar-title>
        <q-tabs>
          <!-- <q-route-tab :to="`${getLanguageFromPath()}/`" label="Home" />
          <q-route-tab :to="`${getLanguageFromPath()}/about`" label="About" /> -->
          <q-route-tab v-for="p in pages" v-bind:key="p.id" :to="p.path" class="q-ml-md">
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
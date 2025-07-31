<script setup lang="ts">
const route = useRoute()

// ISO 3166-1 alpha-2
const LANGUAGES = ['en', 'de', 'rs']

const { data: allPages } = await useAsyncData('nav-' + route.path, async () => {
  const allPages = await queryCollection('content').all()
  return allPages.filter(p => p.meta.inNav)
})

const pagesUnsorted = getLanguageFromPath() === 'en'
  ? allPages.value?.filter(p => getLanguageFromPath(p.path) === 'en')
  : allPages.value?.filter(p => getLanguageFromPath(p.path) !== 'en')

const pages = pagesUnsorted?.filter(item => typeof item.meta.navSpot === 'number')
  .sort((a, b) => (a.meta.navSpot as number) - (b.meta.navSpot as number))
  .concat(pagesUnsorted.filter(item => typeof item.meta.navSpot !== 'number'))

function getLanguageFromPath(path = route.path): string {
  for (const l of LANGUAGES) {
    if (path.split('/')[1]?.includes(l))
      return l
  }
  // return LANGUAGES[0]!
  return 'en'
}

function getPathWithoutLanguage(path = route.path): string {
  if (getLanguageFromPath() === 'en') return path.split('/').slice(1).join('/')
  // const pathWithoutLang = path.split('/').slice(2).join('/')
  // if (pathWithoutLang === '/') return ''
  // return pathWithoutLang
  return path.split('/').slice(2).join('/')
}
</script>

<template>
  <q-header class="nav-header q-py-lg text-black" elevated>
    <Container>
      <q-toolbar class="flex-wrap no-ellipsis">
        <q-toolbar-title>
          <a href="/" class="site-title">
            <q-avatar>
              <img src="https://cdn.quasar.dev/logo-v2/svg/logo.svg">
            </q-avatar>
            <span class="q-ml-sm">Talon's Nuxt Playground</span>
          </a>
        </q-toolbar-title>
        <q-tabs>
          <q-route-tab v-for="p in pages" v-bind:key="p.id" :to="p.path" class="q-mr-md">
            <div class="q-tab__label">
              <span v-if="p.meta.name">{{ p.meta.name }}</span>
              <span v-else><abbr title="This page is missing the 'name' property.">???</abbr></span>
            </div>
          </q-route-tab>
          <q-btn-dropdown auto-close stretch flat :label="getLanguageFromPath()">
            <q-list>
              <div v-for="l in LANGUAGES" v-bind:key="l">
                <q-route-tab :label="l.toUpperCase()"
                  :to="l !== 'en' ? `/${l}/${getPathWithoutLanguage()}` : `/${getPathWithoutLanguage()}`"
                  v-if="l !== getLanguageFromPath()" />
              </div>
            </q-list>
          </q-btn-dropdown>
        </q-tabs>
      </q-toolbar>
    </Container>
  </q-header>
</template>

<style lang="scss" scoped>
.nav-header {
  background-color: hsl(206deg 47% 100% / 50%);
  backdrop-filter: blur(16px);
}

.site-title {
  color: black;
  text-decoration: none;

  &:hover {
    text-decoration: underline;
  }
}
</style>
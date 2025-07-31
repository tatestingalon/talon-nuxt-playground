<script setup lang="ts">
const route = useRoute()

const { data: page } = await useAsyncData('page-' + route.path, () => {
  return queryCollection('content').path(route.path).first()
})

if (!page.value) {
  throw createError({ statusCode: 404, statusMessage: 'Seite nicht gefunden / Page not found', fatal: true })
}
</script>

<template>
  <q-layout view="hHh lpR fff">
    <Header />

    <q-page-container>
      <q-page>
        <ContentRenderer v-if="page" :value="page" class="text-body1" />
      </q-page>
    </q-page-container>

    <Footer />
  </q-layout>
</template>

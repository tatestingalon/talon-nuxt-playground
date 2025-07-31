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
      <!-- <q-page class="q-pa-md row justify-center"> -->
      <q-page class="q-py-xl">
        <Container>
          <!-- {{ page }} -->
          <ContentRenderer v-if="page" :value="page" />
        </Container>
      </q-page>
    </q-page-container>

    <Footer />
  </q-layout>
</template>

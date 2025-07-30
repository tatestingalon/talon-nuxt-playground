<script setup lang="ts">
const route = useRoute()

const { data: page } = await useAsyncData('page-' + route.path, () => {
  return queryCollection('content').path(route.path).first()
})

if (!page.value) {
  throw createError({ statusCode: 404, statusMessage: 'Page not found', fatal: true })
}
</script>

<template>
  <q-layout view="hHh lpR fFf">
    <!-- <HeaderComp
      @updateAndReload="updateAndReload"
      v-if="!$q.screen.lt.sm"
      :update="update"
    /> -->

    <!-- <LeftDrawerComp
      @updateAndReload="updateAndReload"
      v-model="leftDrawerOpen"
      :update="update"
    /> -->

    <q-page-container>
      <q-page class="q-pa-md row justify-center">
        <!-- <RouterView /> -->
        <ContentRenderer v-if="page" :value="page" />
      </q-page>
    </q-page-container>

    <!-- <FooterComp
      @toggleLeftDrawer="toggleLeftDrawer"
      v-if="$q.screen.lt.sm"
      :update="update"
    /> -->
  </q-layout>
</template>

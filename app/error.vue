<script setup lang="ts">
import type { NuxtError } from '#app'

const props = defineProps({
  error: Object as () => NuxtError
})
</script>

<template>
  <q-layout view="hHh lpR fff">
    <Header />

    <q-page-container>
      <q-page class="q-py-xl row justify-center text-center">
        <Container>
          <h1>{{ error?.statusCode }}</h1>
          <p>{{ error?.statusMessage }}</p>
          <!-- <NuxtLink to="/">Go back home</NuxtLink> -->
        </Container>
      </q-page>
    </q-page-container>

    <Footer />
  </q-layout>

</template>

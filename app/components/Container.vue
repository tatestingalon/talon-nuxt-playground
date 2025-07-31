<template>
  <div class="container">
    <slot />
  </div>
</template>

<style lang="scss" scoped>
.container {
  margin-right: auto;
  margin-left: auto;
  width: 100%;
  max-width: 1320px;

  @media (width >= 576px) {
    max-width: 540px;
  }

  @media (width >= 768px) {
    max-width: 720px;
  }

  @media (width >= 992px) {
    max-width: 960px;
  }

  @media (width >= 1200px) {
    max-width: 1140px;
  }

  @media (width >= 1400px) {
    max-width: 1320px;
  }
}
</style>
<template>
  <NuxtRouteAnnouncer />
  <NuxtPage />
</template>

<style lang="scss">
body {
  background-color: beige;
}

h1,
h2,
h3,
h4,
h5,
h6 {
  a {
    color: inherit;
    text-decoration: none;

    &:hover {
      text-decoration: underline;
    }
  }
}

/* .q-tab__indicator {
  display: none;
} */

.align-start {
  align-items: start;
}

.flex-wrap,
.flex-wrap * {
  flex-wrap: wrap;
}

.no-ellipsis,
.no-ellipsis * {
  text-overflow: unset;
  white-space: unset;
  overflow: unset;
}

table {
  border: 2px solid rgb(140 140 140);

  th,
  td {
    border: 1px solid rgb(160 160 160);
    padding: 8px 10px;
  }
}
</style>
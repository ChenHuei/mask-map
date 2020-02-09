<template>
  <div id="app">
    <router-view />
  </div>
</template>

<style lang="scss" scoped>
@import './styles/imports';

#app {
  @include size(100vw, 100vh);
}
</style>

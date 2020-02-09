<template>
  <div class="home">
    <Sidebar class="home-sidebar" />
    <LeafMap class="home-map" />
  </div>
</template>

<script>
import Sidebar from '@/components/Sidebar'
import LeafMap from '@/components/LeafMap'

export default {
  name: 'Home',
  components: { Sidebar, LeafMap }
}
</script>

<style lang="scss" scoped>
@import '@/styles/imports';

.home {
  @include size(100%);
  @include flexCenter;

  &-sidebar {
    @include size(320px, 100%);
  }

  &-map {
    @include size(calc(100% - 320px), 100%);
  }
}
</style>

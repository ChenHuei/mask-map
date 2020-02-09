<template>
  <div class="home">
    <Sidebar class="home-sidebar" :stores="stores" />
    <LeafMap class="home-map" :stores="stores" />
    <Loading v-if="isLoading" />
  </div>
</template>

<script>
import { GET_DATA_API } from '@/constants/api'
import Sidebar from '@/components/Sidebar'
import LeafMap from '@/components/LeafMap'
import Loading from '@/components/Loading'

export default {
  name: 'Home',
  components: { Sidebar, LeafMap, Loading },
  data() {
    return {
      isLoading: false,
      stores: []
    }
  },

  mounted() {
    this.isLoading = true
    fetch(GET_DATA_API)
      .then(res => res.json())
      .then(res => {
        this.stores = res.features
        this.isLoading = false
      })
  }
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

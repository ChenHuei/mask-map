<template>
  <div class="leaf-map">
    <l-map :zoom="zoom" :center="center">
      <l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>
      <l-marker :lat-lng="marker">
        <l-popup :content="text"></l-popup>
      </l-marker>
    </l-map>
  </div>
</template>

<script>
import { LMap, LTileLayer, LMarker, LPopup } from 'vue2-leaflet'
import settings from '@/constants/map.json'

const L = window.L

export default {
  name: 'LeafMap',
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LPopup
  },
  data() {
    return {
      center: L.latLng(settings.center),
      zoom: settings.zoom,
      url: settings.mapURL,
      attribution: settings.attribute,
      marker: L.latLng(settings.center),
      text: 'this is a marker'
    }
  }
}
</script>

<style lang="scss" scoped>
@import '@/styles/imports';

.leaf-map {
  @include size(100%);
}
</style>

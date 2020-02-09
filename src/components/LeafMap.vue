<template>
  <div class="leaf-map">
    <l-map :zoom="zoom" :center="center">
      <l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>
      <l-marker ref="nowLocation" :lat-lng="center" :icon="icon"> </l-marker>
      <v-marker-cluster :options="clusterOptions">
        <l-geo-json
          v-for="store in stores"
          :ref="store.id"
          :key="store.id"
          :geojson="store"
          :options="geoJsonOptions"
        ></l-geo-json>
      </v-marker-cluster>
    </l-map>
  </div>
</template>

<script>
import { LMap, LTileLayer, LMarker, LGeoJson } from 'vue2-leaflet'
import Vue2LeafletMarkercluster from 'vue2-leaflet-markercluster'
import settings from '@/constants/map.json'

const L = window.L

export default {
  name: 'LeafMap',
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LGeoJson,
    'v-marker-cluster': Vue2LeafletMarkercluster
  },
  props: {
    stores: {
      type: Array,
      default: () => {
        return []
      }
    }
  },
  data() {
    return {
      center: L.latLng(settings.center),
      zoom: settings.zoom,
      url: settings.mapURL,
      attribution: settings.attribute,
      icon: L.icon(settings.icon),
      clusterOptions: {
        disableClusteringAtZoom: 16
      },
      geoJsonOptions: {
        pointToLayer: this.setMarkerIcon,
        onEachFeature: (feature, layer) => {
          layer.bindPopup(this.setPopupInformation(feature.properties))
        }
      },
      keys: {}
    }
  },
  mounted() {
    this.$nextTick(() => {
      navigator.geolocation.getCurrentPosition(pos => {
        const { latitude, longitude } = pos.coords
        this.center = [latitude, longitude]

        this.$refs.nowLocation.mapObject.bindTooltip('You', {
          offset: [0, -36],
          permanent: true,
          direction: 'top'
        })
      })
    })
  },
  methods: {
    setMarkerIcon(feature, latlng) {
      const store = feature.properties
      const { mask_adult, mask_child } = store
      const color = mask_adult + mask_child === 0 ? 'grey' : 'blue'
      const icon = L.icon({
        ...settings.icon,
        iconUrl: `https://raw.githubusercontent.com/pointhi/leaflet-color-markers/master/img/marker-icon-2x-${color}.png`
      })
      return L.marker(latlng, { icon })
    },
    setPopupInformation(item) {
      return `
        <h2 class="store-title">${item.name}</h2>
        <a
          class="store-description store-description__address"
          style="display: block;"
          target="_blank"
          href="https://www.google.com.tw/maps/place/${item.address}"
        >
          ${item.address}
        </a>
        <a
          class="store-description store-description__phone"
          href="tel:+886-${item.phone.substring(1).replace(' ', '')}"
        >
          ${item.phone}
        </a>
        <hr>
        <div>成人口罩: ${item.mask_adult}</div>
        <div>小孩口罩: ${item.mask_child}</div>
        <div>更新時間: ${item.updated === '' ? '(不明)' : item.updated}</div>
      `
    },

    convertAddress(address) {
      return `https://www.google.com.tw/maps/place/${address}`
    },
    convertPhone(phone) {
      return `tel:+886-${phone.substring(1).replace(' ', '')}`
    }
  }
}
</script>

<style lang="scss">
@import '@/styles/imports';
@import '~leaflet.markercluster/dist/MarkerCluster.css';
@import '~leaflet.markercluster/dist/MarkerCluster.Default.css';

.leaf-map {
  @include size(100%);
}
</style>

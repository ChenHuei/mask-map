<template>
  <section class="sidebar">
    <transition>
      <Loading v-if="isLoading" />
    </transition>
    <div class="sidebar-filter">
      <el-select v-model="city" @change="resetDistrict">
        <el-option
          v-for="city in Object.keys(AREA)"
          :key="city"
          :value="city"
        ></el-option>
      </el-select>
      <el-select v-model="district" placeholder="請選擇">
        <el-option
          v-for="district in AREA[city]"
          :key="district"
          :value="district"
        ></el-option>
      </el-select>
      <el-select v-model="type">
        <el-option
          v-for="type in MASK_TYPE"
          :key="type.value"
          :label="type.label"
          :value="type.value"
        ></el-option>
      </el-select>
      <p class="sidebar-description">
        能夠取得口罩的藥局共有
        <span class="sidebar-description__number">{{ storesAvailable }}</span>
        家
      </p>
    </div>
    <div class="sidebar-list">
      <div
        class="sidebar-store"
        v-for="store in storesFilter"
        :key="store.properties.id"
        @click="$emit('click', store)"
      >
        <div class="sidebar-store__title">
          {{ store.properties.name }}
        </div>
        <a
          class="sidebar-store__description sidebar-store__address"
          target="_blank"
          :href="convertAddress(store.properties.address)"
        >
          {{ store.properties.address }}
        </a>
        <a
          class="sidebar-store__description sidebar-store__phone"
          :href="convertPhone(store.properties.phone)"
        >
          {{ store.properties.phone }}
        </a>
      </div>
    </div>
  </section>
</template>

<script>
import Loading from '@/components/Loading'
import { AREA } from '@/constants/area'
import { MASK_TYPE } from '@/constants'

export default {
  name: 'Sidebar',
  components: { Loading },
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
      isLoading: false,
      city: '台北市',
      district: '',
      type: 'mask',
      MASK_TYPE,
      AREA
    }
  },
  computed: {
    storesFilter() {
      return this.stores.filter(item => {
        const address = item.properties.address
        return (
          address.indexOf(this.city) > -1 && address.indexOf(this.district) > -1
        )
      })
    },
    storesAvailable() {
      return this.storesFilter.reduce((acc, item) => {
        return item.properties[this.type] > 0 ? acc + 1 : acc
      }, 0)
    }
  },
  methods: {
    resetDistrict() {
      this.district = ''
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

<style lang="scss" scoped>
@import '@/styles/imports';

.sidebar {
  overflow: scroll;

  &-filter {
    @include size(100%, auto);
    padding: 16px;
    background-color: color(grey);
    box-sizing: border-box;
  }

  &-description {
    margin: 0;

    &__number {
      font-size: 24px;
      font-weight: 700;
      color: color(primary);
    }
  }

  &-store {
    @include size(100%, auto);
    padding: 24px 16px;
    border-bottom: 2px solid color(grey);
    cursor: pointer;

    &:hover {
      background-color: color(grey);
    }

    &__title {
      margin-bottom: 8px;
      font-size: 20px;
      font-weight: 500;
    }

    &__description {
      @include flexCenter(flex-start);
      margin-bottom: 4px;
      color: color(grey-text);
    }

    &__address::before,
    &__phone::before {
      content: '';
      display: flex;
    }

    &__address::before {
      @include size(16px);
      @include backgroundImage('../assets/location.svg');
      margin-right: 4px;
    }

    &__phone::before {
      @include size(12px);
      @include backgroundImage('../assets/phone.svg');
      margin-right: 8px;
    }
  }
}
.el-select {
  @include size(100%, auto);
  margin-bottom: 12px;

  &:last-child {
    margin-bottom: 0;
  }
}
</style>

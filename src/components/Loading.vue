<template>
  <div class="loading-mask">
    <div class="loading-icon"></div>
  </div>
</template>

<style lang="scss" scoped>
@import '@/styles/imports';

.loading {
  &-mask {
    @include size(100vw, 100vh);
    @include flexCenter;
    @include fixed;
    background-color: color(mask);
    z-index: 1000;
  }

  &-icon {
    @include size(120px);
    @include backgroundImage('../assets/Loading.svg');
    position: relative;

    &::after {
      content: 'Loading';
      position: absolute;
      bottom: 0;
      left: 50%;
      color: color(white);
      transform: translateX(-50%);
    }
  }
}
</style>

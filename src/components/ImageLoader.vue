<template>
  <div class="image-loading-spinner" v-if="isloadingScannedImage">
    <div class="spinner">
    </div>
  </div>
</template>

<script>
export default {
  name: 'ImageLoader',
  props: {
    isloadingScannedImage: Boolean,
  },
};
</script>

<style>
.image-loading-spinner {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 300px;
}

.spinner {
  border-radius: 50%;
  width: 3rem;
  height: 3rem;
  border: 0.4rem solid rgba(255, 255, 255, 0.2);
  border-top-color: #fff;
  animation: spinner 0.8s linear infinite;
}

@keyframes spinner {
  to {
    transform: rotate(360deg);
  }
}

</style>

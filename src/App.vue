<template>
  <div class="magnify">
    <div ref="imageRef" class="magnify__image" @mousemove="imageMoveHandler">
      <img :src="image" alt="image">
      <div
          class="mag"
          :style="`--scale: ${scale};left: ${magX}%;top: ${magY}%;`"
      />
    </div>
    <div class="magnify__popup">
      <img
          :src="image"
          :style="`transform: scale(${scale}) translate(${translateX}%, ${translateY}%)`"
          alt="image"
      >
    </div>
  </div>
</template>

<script lang="ts" setup>
import {ref} from "vue";

const image = ref<string>('/main-image.jpg');
const imageRef = ref<HTMLDivElement | null>(null)
const scale = ref<number>(4);
const translateX = ref<number>(0);
const translateY = ref<number>(0);
const magX = ref<number>(0)
const magY = ref<number>(0)

const imageMoveHandler = (event) => {
  const clientX = event.clientX;
  const clientY = event.clientY;

  const {
    top,
    left,
    width,
    height
  } = imageRef.value.getBoundingClientRect();

  const maxXPercent = 1 - (width / (width * scale.value));
  const maxYPercent = 1 - (height / (height * scale.value));
  const toLeft = ((clientX - left) / width * maxXPercent) * 100;
  const toTop = ((clientY - top) / height * maxYPercent) * 100;

  translateX.value = toLeft * -1;
  translateY.value = toTop * -1;

  magX.value = toLeft;
  magY.value = toTop;
}

</script>

<style lang="scss" scoped>
.magnify {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
  max-width: 75%;
  margin: 0 auto;

  &__image {
    position: relative;
    cursor: none;

    img {
      width: 100%;
      height: 100%;
    }
  }

  &__popup {
    overflow: hidden;

    img {
      transform-origin: top left;
      width: 100%;
      height: 100%;
    }
  }
}

.mag {
  position: absolute;
  width: calc(100% / var(--scale));
  height: calc(100% / var(--scale));
  border: 3px solid #000;
}
</style>

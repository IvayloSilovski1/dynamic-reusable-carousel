<template>
  <div class="home">
    <Carouse
      :navigation="true"
      :pagination="false"
      :startAutoPlay="true"
      :timeout="5000"
      class="carousel"
      v-slot="{ currentSlide }"
    >
      <Slide v-for="(slide, index) in CarouselSlides" :key="index">
        <div class="slide-info" v-show="currentSlide === index + 1">
          <img :src="require(`../assets/${slide}.jpg`)" alt="" />
        </div>
      </Slide>
    </Carouse>
  </div>
</template>

<script>
// @ is an alias to /src
import Carouse from "@/components/Carouse.vue";
import Slide from "@/components/Slide.vue";

export default {
  name: "Home",
  components: {
    Carouse,
    Slide,
  },

  setup() {
    const CarouselSlides = ["bg-1", "bg-2", "bg-3"];

    return { CarouselSlides };
  },
};
</script>

<style lang="scss" scoped>
.carousel {
  position: relative;
  max-height: 100vh;
  height: 100vh;
}

.slide-info {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  max-height: 100%;
  height: 100%;

  img {
    min-width: 100%;
    height: 100%;
    object-fit: cover;
  }
}
</style>

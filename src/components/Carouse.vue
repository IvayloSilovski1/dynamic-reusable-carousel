<template>
  <div class="carousel">
    <!-- default slot -->
    <slot :currentSlide="currentSlide" />

    <!-- Navigation -->
    <div v-if="navEnabled" class="navigate">
      <div @click="prevSlide" class="toggle-page left">
        <i class="fas fa-chevron-left"></i>
      </div>
      <div @click="nextSlide" class="toggle-page right">
        <i class="fas fa-chevron-right"></i>
      </div>
    </div>

    <!-- Pagination -->
    <div v-if="paginationEnabled" class="pagination">
      <span
        v-for="(slide, index) in getSlideCount"
        :key="index"
        @click="selectSlide(index)"
        :class="{ active: index + 1 === currentSlide }"
      >
      </span>
    </div>
  </div>
</template>

<script>
import { ref, onMounted } from "vue";

export default {
  name: "Carouse",
  props: ["startAutoPlay", "timeout", "navigation", "pagination"],
  setup(props) {
    const currentSlide = ref(1);
    const getSlideCount = ref(null);
    const autoPlayEnabled = ref(
      props.startAutoPlay === undefined ? true : props.startAutoPlay
    );
    const timeoutDuration = ref(
      props.timeout === undefined ? 5000 : props.timeout
    );
    const paginationEnabled = ref(
      props.pagination === undefined ? true : props.pagination
    );
    const navEnabled = ref(
      props.navigation === undefined ? true : props.navigation
    );

    // next slide
    const nextSlide = () => {
      if (currentSlide.value === getSlideCount.value) {
        currentSlide.value = 1;

        return;
      }
      currentSlide.value += 1;
    };

    // prev slide
    const prevSlide = () => {
      if (currentSlide.value === 1) {
        currentSlide.value = getSlideCount.value + 1;
      }
      currentSlide.value -= 1;
    };

    const selectSlide = (slideIndex) => {
      if (currentSlide.value === slideIndex + 1) return; // not necessary
      currentSlide.value = slideIndex + 1;
    };

    // autoplay
    const autoplay = () => {
      setInterval(() => {
        nextSlide();
      }, timeoutDuration.value);
    };

    if (autoPlayEnabled.value) {
      autoplay();
    }

    onMounted(() => {
      getSlideCount.value = document.querySelectorAll(".slide").length;
    });

    return {
      currentSlide,
      getSlideCount,
      nextSlide,
      prevSlide,
      selectSlide,
      paginationEnabled,
      navEnabled,
    };
  },
};
</script>

<style scoped lang="scss">
.navigate {
  position: absolute;
  padding: 0 1rem;
  height: 100%;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;

  .toggle-page {
    display: flex;
    flex: 1;
  }
  .right {
    justify-content: flex-end;
  }

  i {
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 50%;
    width: 3.5rem;
    height: 3.5rem;
    font-size: 1.5rem;
    background-color: #6347c7;
    color: #fff;
  }
}

.pagination {
  position: absolute;
  bottom: 2.5rem;
  width: 100%;
  display: flex;
  gap: 1rem;
  justify-content: center;
  align-items: center;

  span {
    cursor: pointer;
    width: 1rem;
    height: 1rem;
    border-radius: 50%;
    background-color: #fff;
    box-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.1), 0 1px 2px 0 rgba(0, 0, 0, 0.06);
  }

  .active {
    background-color: #6347c7;
  }
}
</style>

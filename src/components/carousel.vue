<template>
   <div class="carousel">
      <slot :currentSlide="currentSlide" />

      <!-- navigation -->
      <div v-if="navEnabled" class="navigate">
         <div class="icons left">
            <h3 @click="prevSlide">LEFT</h3>
         </div>
         <div class="icons right">
            <h3 @click="nextSlide">RIGHT</h3>
         </div>
      </div>
      <!-- pagination -->
      <div v-if="paginationEnabled" class="pagination">
         <span
            @click="gotoSlide(index)"
            v-for="(slide, index) in getslideCount"
            :key="index"
            :class="{ active: index + 1 === currentSlide }"
         >
         </span>
      </div>
   </div>
</template>

<script>
import { onMounted, ref } from "vue";
export default {
   props: ["startAutoplay", "timeout", "navigation", "pagination"],
   setup(props) {
      const currentSlide = ref(1);
      const getslideCount = ref(null);
      const autoPlayEnabled = ref(
         props.startAutoplay === undefined ? true : props.startAutoplay
      );
      const timeoutDuration = ref(props.timeout === undefined ? 5000 : props.timeout);
      const paginationEnabled = ref(
         props.pagination === undefined ? true : props.pagination
      );
      const navEnabled = ref(props.navigation === undefined ? true : props.navigation);

      // next slide
      const nextSlide = () => {
         if (currentSlide.value === getslideCount.value) {
            currentSlide.value = 1;
            return;
         }
         currentSlide.value += 1;
      };
      // prev slide
      const prevSlide = () => {
         if (currentSlide.value === 1) {
            currentSlide.value = 1;
            return;
         }
         currentSlide.value -= 1;
      };

      // pagination
      const gotoSlide = (index) => {
         currentSlide.value = index + 1;
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
         getslideCount.value = document.querySelectorAll(".slide").length;
      });

      return {
         currentSlide,
         nextSlide,
         prevSlide,
         getslideCount,
         gotoSlide,
         autoplay,
         paginationEnabled,
         navEnabled,
      };
   },
};
</script>

<style scoped>
.navigate {
   padding: 0 16px;
   max-height: 100%;
   width: 100%;
   position: absolute;
   display: flex;
   justify-content: center;
   align-items: center;
}

h3 {
   padding: 5px;
   border-radius: 5px;
   border: 1px solid;
   width: fit-content;
   cursor: pointer;
}

.icons {
   display: flex;
   flex: 1;
}

.right {
   justify-content: flex-end;
}

.pagination {
   position: absolute;
   width: 100%;
   bottom: 24px;
   display: flex;
   align-items: center;
   justify-content: center;
   gap: 16px;
}
span {
   cursor: pointer;
   width: 20px;
   height: 20px;
   max-height: 20px;
   border-radius: 50%;
   background-color: orange;
}
.active {
   background-color: teal;
}
</style>

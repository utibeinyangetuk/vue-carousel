<template>
   <div class="home">
      <carousel
         :navigation="false"
         :pagination="true"
         :startAutoPlay="true"
         class="carousel"
         v-slot="{ currentSlide }"
      >
         <slide v-for="(slide, index) in carouselSlides" :key="index">
            <div v-show="currentSlide === index + 1" class="slideInfo">
               <img :src="require(`./assets/images/${slide}.png`)" alt="#slide images" />
            </div>
         </slide>
      </carousel>
   </div>
</template>

<script>
import carousel from "../src/components/carousel.vue";
import slide from "../src/components/slide.vue";
export default {
   name: "Home",
   components: { carousel, slide },
   setup() {
      const carouselSlides = ["image1", "image2", "image3"];

      return { carouselSlides };
   },
};
</script>

<style scoped>
.carousel {
   height: 80vh;
   max-height: 100vh;
   border-radius: 5px;
   position: relative;
}
.slideInfo {
   position: absolute;
   top: 0;
   left: 0;
   width: 100%;
   max-height: 100%;
}

img {
   border-radius: 5px;
   max-width: 1000px;
   max-height: 70vh;
   align-self: center;
}
</style>

<style>
* {
   box-sizing: border-box;
   text-align: center;
   margin-top: 20px;
}
</style>

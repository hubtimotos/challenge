<script setup lang="ts">
import { ref, defineProps, onMounted } from 'vue'
import axios from 'axios' // Importiere axios für HTTP-Anfragen

const props = defineProps({
  images: Object
})

const imageCount = ref(0)

onMounted(async () => {
  try {
    const response = await axios.get(`http://localhost:3000/images/count/${props.images.path}`)
    imageCount.value = response.data.count
  } catch (error) {
    console.error('Fehler beim Abrufen der Bildanzahl:', error)
  }
})
</script>

<template>
  <div class="image-slider">
    <img
      v-for="index in imageCount"
      :key="index"
      :class="{ active: currentSlide === index }"
      :data-slider="index"
      :src="`http://localhost:3000/images/${props.images.path}/${props.images.colorPathName}${index}.webp`"
      alt="#"
    />
    <div class="slider-bullets">
      <span
        v-for="index in imageCount"
        :key="index"
        :class="{ bullet: true, active: currentSlide === index }"
        :data-slider="index"
        @click="changeSlide(index)"
      ></span>
    </div>
  </div>
</template>

<script lang="ts">
const currentSlide = ref(0)
const changeSlide = (index: number) => {
  console.log(currentSlide, index)
  currentSlide.value = index
}
</script>

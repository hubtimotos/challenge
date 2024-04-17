<script setup lang="ts">
import { ref, defineProps, watch } from 'vue'
import ItemDetail from '@/components/product/ItemDetail.vue'

const props = defineProps({
  products: Object 
});

const showModal = ref(false)
const selectedColor = ref(0)

// Überwache die Änderungen an den Produktdaten
watch(() => props.products, (newValue, oldValue) => {
  if (newValue && newValue.length > 0 && newValue[0].colors.length > 0) {
    selectedColor.value = newValue[0].colors[0]
  }
})

const openModal = () => {
  showModal.value = true
  document.body.classList.add('modal-open')
}
const closeModel = () => {
  showModal.value = false
  document.body.classList.remove('modal-open')
}

const changeColor = (color) => {
  selectedColor.value = color
}
</script>

<template>
  <template v-for="product in props.products" :key="product">
    <div class="product-list-item-wrapper">
      <div class="product-list-item" @click="openModal()">
        <!-- Hier wird selectedColor berücksichtigt -->
        <img :src="`http://localhost:3000/images/${selectedColor.path}/${selectedColor.colorPathName}1.webp`" :alt="product.name" @click="openModal" />
        <div class="product-list-item-content">
          <span class="brand">{{ product.brandId }}</span>
          <h3>{{ product.name }}</h3>
          <p class="price">{{ product.price }}</p>
        </div>
      </div>
      <div class="colors">
        <span v-for="(color, index) in product.colors" :key="index" class="color-circle" @click="changeColor(color)">
          <span class="color-circle-inner" :style="`background-color: ${color.colorHex};`"></span>
        </span>
      </div>
    </div>
    <ItemDetail v-if="showModal" :product="product" @close-modal="closeModel" />
  </template>
</template>

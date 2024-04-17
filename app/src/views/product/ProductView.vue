<script setup lang="ts">
import { ref, onMounted } from 'vue';
import ItemList from '@/components/product/ItemList.vue';
import ProductFilter from '@/components/product/ProductFilter.vue';
import productsData from '@/assets/products.json';

const products = ref([]);

onMounted(() => {
  products.value = productsData;
});
</script>

<template>
  <div id="content">
    <div class="container">
      <ProductFilter />
    </div>
    <div class="container">
      <div class="grid">
        <ItemList :products="products" />
      </div>
    </div>
  </div>
</template>

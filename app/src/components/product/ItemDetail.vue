<script setup lang="ts">
import ImageSlider from '@/components/features/ImageSlider.vue'
import ContentTab from '@/components/features/ContentTab.vue'

import { defineProps } from 'vue'

const props = defineProps({
  product: Object
})

const selectedColor = props.product.colors[0]
const selectedColorName = selectedColor
const changeColorName = (color) => {
  selectedColorName.value = color
}
const changeColor = (color) => {
  selectedColor.value = color
  console.log(selectedColor.value)
}
</script>
<template>
  <div class="product-modal-wrapper">
    <div class="product-modal container">
      <div class="product-modal-close" @click="$emit('close-modal')">x</div>
      <div class="product-modal-content">
        <div class="product-modal-front grid">
          <ImageSlider :images="selectedColor" />
          <div class="product-modal-info">
            <span v-if="props.product.isNew" class="age-flag">Brand new</span>
            <h2>{{ props.product.name }}</h2>
            <div class="product-modal-info-wrapper price">
              <p class="price">{{ props.product.price }}</p>
              <img src="#" class="brand" alt="#" />
            </div>
            <div class="product-modal-info-wrapper">
              <p class="color">Color: <strong class="color-name">{{ selectedColorName.name }}</strong></p>
              <div class="icon-wrapper colors">
                <div class="color-image icon" v-for="(color, index) in props.product.colors" :key="index" :class="{ active: selectedColor === index }" @mouseenter="changeColorName(color)" @click="changeColor(color)">
                  <img
                    :src="`http://localhost:3000/images/${color.path}/color_showcase/${color.colorPathName}.webp`"
                    alt="#"
                  />
                </div>
              </div>
            </div>
            <div class="product-modal-info-wrapper">
              <p class="size">Size: <strong class="color-name">SIZEVALUE</strong></p>
              <div class="icon-wrapper sizes">
                <div class="size-pill icon" v-for="(size, index) in props.product.sizes" :key="index">
                  {{ size }}
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="product-modal-desc">
          <ContentTab :tabs="props.product.productDetails">
            <template #tab-0>
              <h4>{{ props.product.productDetails[0].title }}</h4>
              <ul>
                <li v-for="(item, index) in props.product.productDetails[0].content" :key="index">
                  <span class="list-title">{{ item.name }}:</span> {{ item.value }}
                </li>
              </ul>
            </template>
            <template #tab-1>
              <h4>{{ props.product.productDetails[1].title }}</h4>
              <ul>
                <li v-for="(item, index) in props.product.productDetails[1].content" :key="index">
                  <span class="list-title">{{ item.name }}:</span> {{ item.value }}
                </li>
              </ul>
            </template>
            <template #tab-2>
              <div class="material">
                <div class="material-content" v-for="(subcontent, index) in props.product.productDetails[2].content.materialWrap" :key="index">
                  <h4>{{ subcontent.title }}</h4>
                  <ul>
                    <li v-for="(item, index) in subcontent.content" :key="index">
                      <span class="list-title">{{ item.name }}</span>: {{ item.value }}
                    </li>
                  </ul> 
                </div>
              </div>
              <div class="care">
                <h4>{{ props.product.productDetails[2].content.care.title }}</h4>
                <ul>
                  <li v-for="(subcontent, index) in props.product.productDetails[2].content.care.content" :key="index">
                    <i :class="getIconForName(subcontent.name)"></i>: {{ subcontent.value }}
                  </li>
                </ul>
              </div>
            </template>
            <template #tab-3>
              <h4>{{ props.product.productDetails[3].title }}</h4>
              <div class="text" v-if="props.product.productDetails[3].certified === true">
                <p>
                  WE CARE: Items with other sustainable propertiers that go beyond our minimum standard are marked with the WE CARE label.
                </p>
                <h4>ICON Certified sustainable fibre</h4>
                <p>
                  When it comes to certain sustainable fibres, we're commited to using natural firbes from renewable sources. The raw materials are cultivated via resource-saving methods.
                  <small>
                    This product support economically, ecologically and socially sustainable cotton farming.<br>
                    The sourcing if sustainable cotton follows the principal of mass balance. You can find more information <a href="#" alt="">here</a>.
                  </small>
                </p>
              </div>
            </template>
          </ContentTab>
        </div>
      </div>
      <div class="product-modal-footer">
        <h2>{{ props.product.name }}</h2>
        <button class="button button-primary">Add to Cart ICON</button>
      </div>
    </div>
    <div class="product-modal-background" @click="$emit('close-modal')"></div>
  </div>
</template>
<script lang="ts">
  const getIconForName = (name: string) => {
    const iconMap: Record<string, string> = {
      washing: 'washing-machine-icon',
      bleach: 'bleach-icon',
      ironing: 'ironing-icon',
      tumble: 'tumble-icon',
      dryCleaning: 'dry-cleaning-icon'
    }
    return iconMap[name]
  }
</script>
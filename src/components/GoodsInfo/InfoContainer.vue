<template>
  <div class="g-grid_item js-sku-details" id="p-specGridItem">
    <goods-info-two></goods-info-two>
    <div class="g-block-sm js-sku-cutlock"></div>
    <goods-info></goods-info>
  </div>
</template>

<script setup lang="ts">
import goodsInfoTwo from "./GoodsInfoTwo.vue";
import GoodsInfo from "./GoodsInfo.vue";
</script>

<style scoped>
.g-block-sm{
  padding:20px
}
</style>
<template>
<div id = "banner">\
    <item-cat></item-cat>
    <swiper-container></swiper-container>
</div>
</template>

<script lang="ts">
import ItemCat from "../category/ItemCat.vue";
import SwiperContainer from "./SwiperContainer.vue";

export default{
    components:{ItemCat,SwiperContainer},
}
</script>

<style scoped>
#banner{
  width: 1226px;
  display: flex;
  justify-content:space-between;
  margin:10px;
}

</style>
<template>
<div class="p-grid_gallery g-grid_item g-sm-full">
    <div
      class="p-gallery p-gallery-static"
      aria-hidden="true"
      data-breakpoints=""
      captionlength="0"
    >
      <div class="p-gallery_top">
        <div
          class="p-gallery_photo"
          role="button"
          aria-expanded="false"
          aria-controls="p-galleryModal"
          tabindex="0"
        >
        <div class="swiper-container p-gallery_photo_el swiper-container-fade swiper-container-horizontal">
					<div class="swiper-wrapper js-gallery-images">
						<div class="swiper-slide p-gallery_item swiper-slide-active" data-caption="" style="width: 395px; opacity: 1; transform: translate3d(0px, 0px, 0px);">
								<img class="swiper-lazy swiper-lazy-loaded" :src="imgSrc" alt="" style="width: 320px"/>
									</div>
			
							</div>
				</div>
        <div class="swiper-container"
        
        >
             <div class="inner-swiper"
             :style="{
          
          transform:'translate3d( '+ x + 'px,0px,0px)',

        }"
             >
            <div
                class="swiper-slide "
                @click="changeUrl"
                v-for = "(imgs,index) in imgList"
                :key = "index"
                style="width: 350px"  
              >
              
              <img 
                 class="slide-image-div"
                
                 v-for = "(img,idx2) in imgs"
                 :key = "idx2"
                 :src ="img"
              />   
            </div>
            
            </div>
            <div class="p-gallery_controls">
				<div class="p-gallery_btn p-gallery_prev swiper-button-disabled"
        >
          <span
          
          v-if="x!==0"
          class="material-symbols-outlined"
          style="height: 0px; color: #009e96"
          @click="rightShift"   
        >
          arrow_back_ios
        </span>
        <span
          v-if="x===0"
          class="material-symbols-outlined"
          style="height: 0px; color: gray"
          @click="rightShift"  
           
        >
          arrow_back_ios
        </span>
        

        </div>
				<div class="p-gallery_pagination 
                swiper-pagination-clickable swiper-pagination-bullets"
        >
        <span
              v-for="(n,indexA) in imgList.length"
              :key="indexA"
              class="swiper-pagination-bullet"
              :class="{'swiper-pagination-bullet-active': index == -indexA}"
              @click="changeDiv(indexA)"
            > 
            </span>
          <!-- <span class="swiper-pagination-bullet swiper-pagination-bullet-active"
          ></span>
          <span class="swiper-pagination-bullet"></span> -->
          </div>
				<div
        class="p-gallery_btn p-gallery_next"
        
      >
        <span 
        :class="x===max?'gray':'green'"
        class="material-symbols-outlined" 
        
        @click="leftShift"
        > 
        arrow_forward_ios 
        </span>
        <!-- <span 
        v-if="x===max"
        class="material-symbols-outlined" 
        style=" color: grey" 
        @click="leftShift"
        > 
        arrow_forward_ios 
        </span> -->

      </div>
			</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>


<script setup lang="ts">
import { useStore } from "../../store/index";
import { useRoute } from "vue-router";
import { onMounted, computed } from "vue";
// import { data } from "dom7";

const store = useStore();
const route = useRoute();
const goodsId = route.params.goodsId; 

onMounted(() => {
  store.dispatch("setSizeList",goodsId);
});

let imgList = computed(() => store.getters.getImgList);
console.log("qqqqqqqq",imgList)

let imgSrc = computed(() => store.getters.getImgSrc);
const changeUrl = (e: Event) => {
  if (e.target instanceof HTMLImageElement) {
    store.commit("changeUrl", e.target.src);
  }
};

// const rightShift = (e:Event) => {
//   if (e.target instanceof HTMLImageElement) {
//     store.commit("rightShift", e.target.src);
//   }
// }
// function leftShift(){
 
//   let top = document.querySelector(".inner-swiper") as HTMLElement;
//   top.style.transform = "translate3d(-300px,0px,0px)";
// }
// function rightShift(){
//   let top = document.querySelector(".inner-swiper") as HTMLElement;
//   top.style.transform = "translate3d(0px,0px,0px)";
// }
let index = computed(() => store.getters.getIndex);

const max = computed (() => -(imgList.value.length-1)*360);


let x = computed(() =>{
  return ((index.value)*360)
});

// $("#material-symbols-outlined").attr('disabled',true);

function leftShift() {
  store.commit("leftShift");

}
function rightShift() {
  store.commit("rightShift");
}

let changeDiv = (indexA:number) => {
  store.commit("changeDiv",indexA)
}


</script>

<style scoped>

.green-round {
  background: #009e96;
}
.black-round {
  background: black;
}
.material-symbols-outlined {
  font-variation-settings: "FILL" 1, "wght" 400, "GRAD" 0, "opsz" 20;
}

.inner-swiper {
  display: flex;
  /* flex-wrap: wrap; */
}
.swiper-slide {
  width: 400px;
  height: 400px;
}
.slide-image-div {
  width: 100px;
  height: 100px;
  background-size: contain;
  margin: 10px;
  transform:translate3d(10px, 0, 0)
  
}
.p-gallery-static .p-gallery_pagination {
    flex-grow: 1;
    /* text-align: center; */
}
.green{
  color: #009e96;
}
.gray{
  color:#808080;
  pointer-events:none;
}
.g-i-arrow-r, .g-i-arrow-l, .g-i-arrow-u, .g-i-arrow-d, .g-i-arrow-d2 {
    transition: transform 0.3s;
    color:  #009e96;;
}
.p-gallery_btn.swiper-button-disabled .g-i {
    color: #808080;
}
.p-gallery_btn {
    font-size: 2.4rem;
    display: flex;
    width: 44px;
    height: 44px;
    align-items: center;
    justify-content: center;
}
.p-gallery-static .p-gallery_prev {
    transform: translateX(-10px);
}
.p-gallery-static .p-gallery_controls {
    display: flex;
    margin-top: 10px;
    align-items: center;
}
.p-gallery-static .p-gallery_thumbs_item {
    width: calc((100% - 30px) / 4 - 0.1px);
    margin: 10px 10px 0 0;
    padding-bottom: calc((100% - 30px) / 4);
    transition: opacity 0.2s;
    background-repeat: no-repeat;
    background-size: contain;
}
.p-gallery-static .p-gallery_thumbs_item-active {
    box-shadow: 0 0 0 2px  #009e96 n inset;
}
.p-gallery-static .p-gallery_thumbs_item:nth-child(-n + 4) {
    margin-top: 0;
}
.displayMoreGalleryCaption {
    cursor: pointer;
}
p {
    display: block;
    margin-block-start: 1em;
    margin-block-end: 1em;
    margin-inline-start: 0px;
    margin-inline-end: 0px;
}
p, form, h1, h2, h3, h4, h5, h6, ul, ol, dl, dd, input, textarea, select, button {
    margin: 0;
}
.g-align-tc, .g-lg-align-tc {
    text-align: center !important;
}
[aria-hidden='true']:not([data-breakpoints]):not([data-accordion-more]):not(.g-i):not(.g-s) {
    display: none;
}
.p-gallery-static .p-gallery_caption {
    margin-top: 10px;
    word-wrap: break-word;
    overflow-wrap: break-word;
}
.p-gallery_photo_el, .p-galleryReview_photo_el {
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
}
.swiper-container {
  overflow: hidden;
  /* background-color: pink; */
  width:340px;
  
  cursor: pointer;
}

.p-gallery-static {
    max-width: 415px;
}
.p-gallery_thumbs_item {
  background-size: contain;
}
.swiper-slide {
  width: 350px;
  height: 350px;
  display: flex;
  justify-content: flex-start;
}
.silde-image-div {
  width: 100px;
  height: 100px;
  /* background-size: contain;
  background-repeat: no-repeat; */
}
.p-grid_gallery {
  margin: auto !important;
}
.p-gallery_thumbs {
  position: relative;
  margin-top: 15px;
}

.swiper-wrapper {
  position: relative;
  width: 100%;
  height: 100%;
  z-index: 1;
  display: flex;
  transition-property: transform, -webkit-transform;
  box-sizing: content-box;
  transform: translate3d(0, 0, 0);
}
.p-gallery_thumbs {
  display: flex;
  flex-wrap: wrap;
}
.swiper-slide-div {
  display: flex;
  flex-wrap: wrap;
  flex-shrink: 0;
  height: 100%;
  position: relative;
  transition-property: transform;
}
.p-gallery_thumbs_item {
  background-position: center;
}
/* .p-galleryReview_thumbs_item {
  background-position: center;
} */
.p-gallery_thumbs_item:not(.p-gallery_thumbs_item-active) {
  cursor: pointer;
}
.p-gallery_thumbs_item:nth-child(4n) {
  margin-right: 0;
}
.g-i {
  flex-shrink: 0;
  width: 1em;
  height: 1em;
  display: inline-block;
  vertical-align: middle;
  font-style: normal;
  font-weight: normal;
  font-variant: normal;
  line-height: 1;
  letter-spacing: 0;
  text-transform: none;
  pointer-events: none;
  -webkit-font-smoothing: antialiased;
}
.p-gallery_pagination {
  flex-grow: 1;
  text-align: center;
}
.p-gallery .swiper-pagination-bullet-active {
  background:  #009e96;;
}
.p-gallery .swiper-pagination-bullet {
  width: 15px;
  height: 15px;
  margin: 0 5px;
  opacity: 1;
}
.swiper-pagination-clickable {
  cursor: pointer;
}
.swiper-pagination-bullet {
  display: inline-block;
  border-radius: 100%;
}
.p-gallery_next {
  transform: translateX(10px);
}
.g-i-arrow-l {
  transition: transform 0.3s;
  color:  #009e96;;
}
.p-grid .g-grid_item {
  max-width: 395px;
  width: 100% !important;
  margin-top: 40px;
  padding: 0 !important;
}
.g-grid_item {
  max-width: 395px;
  width: 100% !important;
  margin-top: 40px;
  padding: 0 !important;
}
* {
  box-sizing: border-box;
}
.p-gallery_top {
  position: relative;
  margin: auto;
}
.p-gallery_photo[role="button"] {
  transition: opacity 0.2s;
}
.p-gallery_photo {
  position: relative;
  width: 100%;
  height: 0;
  padding-bottom: 100%;
}

.swiper-slide {
  flex-shrink: 0;
  height: 50%;
  position: relative;
}
.p-gallery_item img {
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  max-height: 100%;
  margin: auto;
  width: 100%;
}
.img {
  max-width: 100%;
  vertical-align: bottom;
  border: 0;
  height: auto;
}
.p-gallery_fraction {
  display: none;
}
.p-gallery_fraction {
  line-height: 1;
  position: relative;
  z-index: 1;
  margin: 10px auto;
  padding: 7px 10px;
  color: #fff;
  border-radius: 100px;
  background-color: rgba(0, 0, 0, 0.6);
}
.swiper-wrapper {
    transform: translate3d(0,0,0);
}
.captionArea-static.fixed {
  min-height: 45px !important;
}
.p-gallery_caption {
  margin-bottom: 5px;
}
.p-gallery_thumbs_item:not(.p-gallery_thumbs_item-active) {
  cursor: pointer;
}
.p-gallery_caption {
  margin-top: 10px;
  overflow-wrap: break-word;
}
.p-gallery-static .p-gallery_thumbs .swiper-slide {
    display: flex;
    flex-wrap: wrap;
}

.g-align-tc {
  text-align: center !important;
}

.g-link .g-i:first-child {
  margin-right: 0.35em;
}
.p-gallery_thumbs_item:not(.p-gallery_thumbs_item-active) {
  cursor: pointer;
}
.g-link [class*="g-i-arrow-"] {
  margin-top: -0.2em;
}
.g-link,
.g-lg-link {
  display: inline-flex;
  align-items: center;
}
a {
  text-decoration: none;
  color: #333;
}
.g-i {
  flex-shrink: 0;
  width: 1em;
  height: 1em;
  display: inline-block;
  vertical-align: middle;
  font-style: normal;
  font-weight: normal;
  font-variant: normal;
  line-height: 1;
  letter-spacing: 0;
  text-transform: none;
  pointer-events: none;
  -webkit-font-smoothing: antialiased;
}

.g-link [class*="g-i-arrow-"] {
  margin-top: -0.2em;
}
.g-link .g-i:first-child {
  margin-right: 0.35em;
}

.material-symbols-outlined{
  cursor: pointer;
}
.swiper-container {
    margin: 0 auto;
    position: relative;
    overflow: hidden;
    list-style: none;
    padding: 0;
    z-index: 1;
}
</style>
<template>
<div class="g-layout_sidebar" style="margin:30px">
 <div class="g-pane g-pane-gray">
				<div class="g-position-r g-units-lg js-sku-sidebar">
					
					<dl class="p-order">
	<dt>納品方法</dt>
		<dd><span>玄関先迄納品（店舗受取可能商品）</span></dd>
	<dt>配送目安</dt>	
		<dd>2～6日で出荷</dd>
	<dt>返品・交換</dt>
		<dd>14日間返品可能<p class="p-order_help">
					<a class="g-link" href="/ec/userguide/cancel/">
						<span>返品・交換について</span>
                        <i class="g-i g-i-info" aria-hidden="true"></i>
					</a>
				</p>
		</dd>
	<dt>送料</dt>
	<dd>
		<span class="g-label-price">有料</span>
        <p class="p-order_help">
            <a class="g-link" href="/ec/userguide/delivery/">
                <span>送料について</span><i class="g-i g-i-info" aria-hidden="true"></i></a></p>
	</dd>
	</dl>

<p><img src="/ecstatic/front/free_shipping2.jpg" alt=""></p>
<dl class="p-pcs">
	<dt>
        <label for="p-pieces">数量</label>
	</dt>
	<dd>
        <input 
		class="g-input g-input-sm addToCartQty" 
		id="p-pieces" 
		type="number" 
		name="quantity" 
		:value="quantity"
		placeholder="1"
		@input="updateQuantity"
		oninput="value = value.replace(/\D-/g , '');
		if ( value.length > 3 ) value = value.slice ( 0,3 ) "
		max="999" min="0" >

		<input 
		class="g-input g-input-sm addToCartQty" 
		type="hidden"
		:value="sku"
		@input="updateQuantity">
		<input 
		class="g-input g-input-sm addToCartQty" 
		type="hidden"
		:value="sizeValue"
		@input="updateQuantity">
		<input 
		class="g-input g-input-sm addToCartQty" 
		type="hidden"
		:value="point"
		@input="updateQuantity">
	</dd>
</dl>

<div>
	<div class="g-flow-0 g-align-fbl">
		<dl class="p-price p-price-area">
			<dd class="g-price g-price-lg g-price-ra price-size-up ">
				{{ price }}<span>円</span></dd>
			</dl>
	</div>
	</div>
			<div class="g-foot-v">
				<div class="cartBtnArea disp">
                    <!-- <a href="/cart/page/989898" style="text-direction:none"> -->
					<button 
					@click="addItem" 
					class="g-btn g-btn-cv g-btn-c g-fw addToCartBtn" 
					id="p-addItem" 
					type="button">
								<i class="g-i g-i-add-cart" aria-hidden="true"></i>
                                <span>カートに入れる</span>
                                </button>
                                <!-- </a> -->
						</div>
			</div>
		<ul class="g-grid-2 g-grid-xs p-misc">
		<li class="g-grid_item p-misc_item">
				<a onclick="jsShopStockClick()" class="g-hover js-popUp-store-inventory" href="#p-stockModal" role="button" aria-expanded="false" data-sku-code="7564862">
				<div class="p-misc_i g-hover_img">
							<i class="g-s g-s-stock-g" aria-hidden="true"></i>
						</div>
					<span class="p-misc_label">店舗在庫を確認</span>
					</a>
			</li>
		<li class="g-grid_item p-misc_item">
				<a :class="Existed ? 'canNotClick' : 'canClick'"
                  @click="intoWish">
					<div class="p-misc_i g-hover_img ">
						<span class="material-symbols-outlined g-s g-s-favorite-g">
                      favorite
                    </span>
					</div>
					<span class="p-misc_label">お気に入り</span>
				</a>
			
				<!-- <a onclick="" id="addFavoriteDiv" class="g-hover" style="display: none;">
					<div class="p-misc_i g-hover_img ">
							<i class="g-s g-s-favorite"></i>
					</div>
					<span class="p-misc_label">お気に入り</span>
				</a> -->
		</li>
	</ul>
          
          </div>	
			</div>
            </div>
</template>

<script setup lang="ts">
import { computed, onMounted,ref} from "vue";
import { useStore } from "../../store/index";
import { useRoute } from "vue-router";
const route = useRoute();
const goodsId = route.params.goodsId;
const userId = route.params.userId;
const store = useStore();
onMounted( async() => {
    store.dispatch("setSizeList",goodsId);
	await store.dispatch("setWishGoodsList", userId);
});
const price = computed(() => store.getters.getNewList1.price)

const addItem = () =>  store.dispatch("addCart")

const quantity = computed (() => store.getters.getQuantity)
const skuName = computed(() => store.getters.getNewList1.skuName )
const sku = computed(() => store.getters.getNewList1.sku )
store.commit("setSku",sku);

const sizeValue = computed(() => store.getters.getNewList1.sizeValue )
store.commit("setSizeValue",sizeValue);

const point = computed(() => store.getters.getNewList1.point )
store.commit("setPoint",point);

const updateQuantity = (e:Event) => {
	if(e.target instanceof HTMLInputElement){
		store.commit("updateQuantity", e.target.value)
	}
}

const newList1 = computed(() => store.getters.getNewList1);
//判断当前sku的商品是否在【お気に入り】列表（wishgoodsList）中，若存在（true），则不能再点击
const allGoodsList = computed(() => store.getters.getAllGoodsList);
let Existed = ref();
 //判断当前sku的商品是否在【お気に入り】列表（wishgoodsList）中
  if (allGoodsList.value.filter((a) => a.skuName === skuName.value).length > 0) {
    Existed.value = true;
  }
// 把当前sku的商品加到【お気に入り】
const intoWish = () => {
  store.dispatch("intoWish", newList1.value);
  Existed.value = true;
};
</script>

<style scoped>
.canClick {
cursor: pointer;
color: #333;
}
.canNotClick {
pointer-events: none;
color: #009e96;
}
.g-pane, .g-lg-pane {
    padding: 50px;
	width:80%
}
dl {
  width: 80%;
  overflow: hidden;
  padding: 0;
  margin: 0;
}
dt {
  float: left;
  width: 50%;
  /* adjust the width; make sure the total of both is 100% */
  padding: 0;
  margin: 0;
}
dd {
  float: left;
  width: 50%;
  /* adjust the width; make sure the total of both is 100% */
  padding: 0;
  margin: 0;
  font-weight: bold;
}
.g-layout_sidebar {
  position: sticky;
  top: 0;
}
#p-addItem {
  font-size: 16px;
  margin: 8px 4px;
  cursor: pointer;
  padding: 15px 32px;
  border-color: #eb6157;
  background-color: #eb6157;
}
.g-pane-gray,
.g-lg-pane-gray {
  background-color: #f7f7f7;
}
.p-sm-full-cls {
  position: -webkit-sticky;
  position: sticky;
  top: 40px;
}
.g-fw,
.g-lg-fw {
  width: 100% !important;
}
.g-btn-c,
.g-lg-btn-c {
  justify-content: center;
}
.g-btn-cv,
.g-btn-cv,
.g-btn,
.g-lg-btn {
  /* font-size: 1.6rem; */
  /* line-height: 1.5; */
  /* font-weight: normal; */
  display: inline-flex;
  padding: 0;
  transition: background-color 0.2s;
  vertical-align: middle;
  border: 1px solid #dbdbdb;
  border-radius: 4px;
  background-color: #fff;
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
}
</style>
<template>
<div id="entryList">
        <div id="wishlistEntryList" class="g-block-sm" v-if="goodsList.length != 0">
            <div class="p-listControl" >
                <label class="g-checkable">
                    <input type="checkbox" class="g-check-check" style="display:flex;width: 20px;"
                    data-checkall="favorite"
                    v-model="state.checked"
                    @change="selectAll"/>

                  <span class="g-checkable_label" style="display:flex">すべて選択</span>

                </label>
            <div class="p-listControl_edit">
                <div>チェックしたものを</div>
                <ul class="g-linkList-lg" style="display:flex">

                <li v-if="wishList.length > 1">
                <a class="g-link g-link-gray" 
                @click=" isShow07 = true" role="button">

                  <span class="material-symbols-outlined" style="display:flex;color:gray">
                  unfold_more_double</span>
                  <span style="margin-top:-23px;margin-left:25px;display:flex"
                   >移動 </span>

                  <span style="color: #dbdbdb;display:flex;margin-left:60px;margin-top: -23px;"> |</span></a
                >
              </li>

                <span class="material-symbols-outlined" style="color:gray">delete</span>
                    <span style="cursor:pointer"
                    @click="isShow05 = true">削除</span>
                </ul>
            </div>
            <!-- module move -->
            <!-- modal05 チェックした商品を削除?-->
        <GDialog v-model="isShow05">
          <div class="modal">
            <div class="g-modal_el">
              <header class="g-modal_head">
                <p class="g-modal_h" id="p-messageModal_h">商品を削除</p>
                <button
                  @click="isShow05 = false"
                  class="g-modal_close"
                  type="button"
                  aria-label="閉じる"
                >
                  <span
                    class="material-symbols-outlined"
                    style="cursor: pointer; color: #ffffff"
                  >
                    close
                  </span>
                </button>
              </header>
              <div class="g-modal_body">
                <p id="modalMessage">
                  チェックした商品{{ state.checkList.length }}つを削除しますか？
                </p>
                <div class="button-delete-div">
                  <button class="button-delete" @click="deleteGoods">
                    <span>削除する</span>
                  </button>
                </div>
              </div>
            </div>
          </div>
        </GDialog>
        <!-- modal06 チェックした商品を削除した-->
        <GDialog v-model="isShow06">
          <div class="modal">
            <div class="g-modal_el">
              <header class="g-modal_head">
                <p class="g-modal_h" id="p-messageModal_h">商品を削除</p>
                <button
                  @click="isShow06 = false"
                  class="g-modal_close"
                  type="button"
                  aria-label="閉じる"
                >
                  <span
                    class="material-symbols-outlined"
                    style="cursor: pointer; color: #ffffff"
                  >
                    close
                  </span>
                </button>
              </header>
              <div class="g-modal_body">
                <p id="modalMessage">お気に入り商品から削除しました。</p>
              </div>
            </div>
          </div>
        </GDialog>
        <!-- modal07 商品を別のリストへ移動?-->
        <GDialog v-model="isShow07">
          <div class="modal">
            <div class="g-modal_el">
              <header class="g-modal_head">
                <p class="g-modal_h" id="p-messageModal_h">
                  商品を別のリストへ移動
                </p>
                <button
                  @click="isShow07 = false"
                  class="g-modal_close"
                  type="button"
                  aria-label="閉じる"
                >
                  <span
                    class="material-symbols-outlined"
                    style="cursor: pointer; color: #ffffff"
                  >
                    close
                  </span>
                </button>
              </header>
              <div class="g-modal_body">
                <p id="modalMessage">
                  商品を移動させるリストを選択してください。
                </p>
                <div class="button-delete-div">

                  <!-- selectableList是modal选择框 -->
                  <select v-model="selectableList"
                  @change="selectableListBySelect"
                  >
                    <option
                      v-for="(c, index) in canMoveList"
                      :key="index"
                      :value="c.listName"
                    >
                      {{ c.listName }}
                    </option>
                  </select>

                  <button
                    :selectableList="selectableList"
                    class="button-delete"
                    @click="
                      moveGoods(selectableList);
                      isShow07 = false;
                    "
                    style="margin-left: 10px"
                  >
                    <span>変更する</span>
                  </button>
                </div>
              </div>
            </div>
          </div>
        </GDialog>
        <!-- modal08 商品を別のリストへ移動した-->
        <GDialog v-model="isShow08">
          <div class="modal">
            <div class="g-modal_el">
              <header class="g-modal_head">
                <p class="g-modal_h" id="p-messageModal_h">
                  商品を別のリストへ移動
                </p>
                <button
                  @click="isShow08 = false"
                  class="g-modal_close"
                  type="button"
                  aria-label="閉じる"
                >
                  <span
                    class="material-symbols-outlined"
                    style="cursor: pointer; color: #ffffff"
                  >
                    close
                  </span>
                </button>
              </header>
              <div class="g-modal_body">
                <p id="modalMessage">お気に入り商品の移動が完了しました。</p>
              </div>
            </div>
          </div>
        </GDialog>

            </div>
        </div>
    </div>
      <div v-if="goodsList.length == 0">
      <div class="g-1234567">
        お気に入り商品が登録されていません。<br></div>
    <div class="g-123456">
        各商品の紹介ページで「お気に入り」を押すと追加できます。</div>
      </div>
<div v-for="wish in goodsList" :key="wish.id" >
<ul id="p-ProductList" class="g-itemList g-itemList-border g-mt-20 g-mb-20">
    <li class="g-itemList_item">
        <div class="g-media g-media-lg g-media-lead g-media-tail p-favoriteItem">
            <div class="g-media_lead g-align-im">
                <div class="g-checkable">
                    <input 
                    type="checkbox" 
                    name="productCheckBox" 
                    :value="wish.id" 
                    v-model="state.checkList">
                    <!-- <span><i class="g-s g-s-checkbox-on g-checkable_on" aria-hidden="true"></i>
                          <i class="g-s g-s-checkbox-off g-checkable_off" aria-hidden="true"></i>
                    </span> -->
                </div>
            </div>
        <div class="g-media_head" style="width: 400px;">
            <a class="g-hover" href="/goods/detail/10195">
                <img class="g-fw-pic" :src="wish.imgUrl" alt="">
            </a>
       <div >
        <div class="g-media_body g-sm-units-xs g-lg-units-sm" >
            <p class="g-media_h" >
                <a href="/goods/detail/10195">{{wish.skuName}}</a>
            </p>
            <p class="g-price">
                {{wish.price * wish.quantity}}
                <span class="g-price-unit">円（税込）</span>
            </p>
            <dl class="g-flow-gm" style="width: 50px;">
                <dt>数量</dt>
                <dd style="width: 25px;">
                    <input 
                    class="g-input g-input-sm addToCartQty7030893"
                      type="number"
                      inputmode="numeric"
                      name="quantity"
                      size="5"
                      maxlength="3"
                      id="p-pieces"
                      style="margin-left: 10px"
                      oninput="value=value.replace(/\D/g, '')"
                      v-model="wish.quantity"
                      @input="updateQuantity"
                      max="999"
                      min="0"
                      >
                </dd>
            </dl>
        
        </div>
        <div class="g-media_tail g-units g-sm-align-tc">
            <div class="g-position-r">
                <div class="cartBtnArea">
                    <!-- error modal -->
                    <div
                      v-if="showError"
                      class="p-itemAdded g-item-add-error"
                      style="
                        bottom: 70.2083px;
                        animation: 1.8s ease 0s 1 normal both running
                          p-itemAddedIn;
                      "
                    >
                      <button
                        @click="showError = false"
                        class="g-modal_close p-modal_button"
                        type="button"
                        aria-label="閉じる"
                      >
                        <span
                          class="material-symbols-outlined"
                          style="cursor: pointer"
                        >
                          close
                        </span>
                      </button>
                      <div>数量は1以上、999以下で設定してください。</div>
                    </div>
                    <div
                      v-if="showError"
                      class="p-itemAdded g-item-add-error"
                      style="
                        bottom: 70.2083px;
                        animation: 1.8s ease 0s 1 normal both running
                          p-itemAddedIn;
                      "
                    >
                      <button
                        @click="isShow = false"
                        class="g-modal_close p-modal_button"
                        type="button"
                        aria-label="閉じる"
                      >
                        <span
                          class="material-symbols-outlined"
                          style="cursor: pointer"
                        >
                          close
                        </span>
                      </button>
                      <div>カートに追加しました</div>
                      <button @click="isShow = false" class="modal-button">
                        <router-link to="/cart">カートを見る</router-link>
                      </button>
                    </div>
                   
                   <button
                      class="g-btn g-btn-cv g-btn-c g-sm-fw g-lg-btn-func addToCartBtn p-addItem"
                      style="margin-left:40px;font-size:1rem"
                      id="p-addItem7030893"
                      type="button"
                      data-sku-code="7030893"
                      data-price-without-tax="818.0"
                      data-category="ジョイントマット・コルクマット"
                      data-category-id="11480"
                      data-product-id="7030841s"
                      data-bundle="false"
                      @click="addItem(wish.skuName)"
                      :sku="wish.skuName"
                    >
                    <div style="display:flex;">
                      <span class="material-symbols-outlined">
                        add_shopping_cart
                      </span>
                      <span class="g-i-add-cart">カートに入れる</span>
                      </div>
                    </button>
                    
				</div>
			</div>
        </div>
        </div>
        </div>
        </div>
    </li>
</ul>
</div>
</template>

<script setup lang="ts">
import {  onMounted, reactive,ref,computed} from "vue";
import { useStore } from "../../store/index";
import { useRoute } from "vue-router";
const route = useRoute();
const userId = route.params.userId;
const store = useStore();
onMounted( async () => {
    await store.dispatch("setWishList",userId);
    store.dispatch("setWishGoodsList", userId);
    // if (canMoveList.value.length > 0) {
    //   anotherName.value = canMoveList.value[0].listName;
    // }
});


// const props=defineProps<{
//     userId:number
//     skuName:string,
//     price:number,
//     imgUrl:string,
//     id:number,
//     quantity:1
// }>();

const showError = ref(false);
const isShow = ref(false);
const isShow05 = ref(false);
const isShow06 = ref(false);
const isShow07 = ref(false);
const isShow08 = ref(false);

const goodsList = computed(() => store.getters.getGoodsList)
const wishList = computed(() => store.getters.getWishList)
// const quantity = computed(() => store.getters.getGoodsList.quantity)
const canMoveList = computed (() => store.getters.getCanMoveList)

//modal里初始状态
const selectableList = computed (() => store.getters.getSelectableList)
// const selectableList = computed (() => store.getters.getCanMoveList[0].listName)
//------------------商品をカートに入れる-----------------------
//store goods quantity
const quantity = computed(() => store.getters.getGoodsList.quantity)
const updateQuantity = (e: Event) => {
  if (e.target instanceof HTMLInputElement) {
    store.commit("updateQuantity", e.target.value);
  }
};

//add goods into cart.
//商品数量在加入购物车后会回到数量1
const addItem = (skuName: string ) => {
  if ( quantity.value < 1 || quantity.value > 999) {
    showError.value = true;
    store.commit("updateQuantity", 1);
  } else {
    store.dispatch("addCart", skuName);
    isShow.value = true;
    store.commit("updateQuantity", 1);
  }
};

const state = reactive({
  checked: false,
  checkList: [],
});

//------------------すべて選択-----------------------
const selectAll = async () => {
  if (state.checked) {
    const checkList1 = goodsList.value.map((wish) => wish.id);
    console.log("checkList1", checkList1);
    state.checkList = checkList1;
    console.log("checkList", state.checkList);
  } else {
    state.checkList = [];
  }
};


//------------------商品をカートに入れる-----------------------
const deleteGoods = () => {
  let id = -1;
  for (let i = 0; i < state.checkList.length; i++) {
    id = state.checkList[i];
    console.log("id", id);
    store.dispatch("deleteGoods", { id, userId });
  }
  isShow05.value = false;
  isShow06.value = true;
  state.checked = false;
};

const moveGoods = (selectableList: string) => {
  let id = -1;
  for (let i = 0; i < state.checkList.length; i++) {
    id = state.checkList[i];
    console.log("id", id);
    store.dispatch("moveGoods", { selectableList, id, userId });
    isShow08.value = true;
    //不能在全选的情况下进行动作，所以不显示false
    state.checked = false;
  }
      state.checkList = [];
};


const selectableListBySelect = (e) => {
  store.commit("selectableListBySelect", e.target.value);
};

</script>

<style scoped>
.g-checkable{
    margin-left: 100px;
}
.g-fw-pic{
    margin-right:800px;
    width: 150px;
    height: 150px;
}
.g-media_h{
    margin-left:200px;
    margin-top:-150px;
    width:500px;
    font-size:1rem
}
.g-price{
    margin-left:200px;
    font-size:1.3rem
}
.g-price-unit{
    font-size:0.8rem
}
.g-flow-gm{
    margin-left: 228px;
    font-size: 1rem;
}
.g-input-addtocart{
    margin-left: 110px;
    /* margin-bottom:60px;
    margin-right: 30px; */
    margin-top:-50px;
    height: 50px;
}
.cartBtnArea{
    margin-left: 200px;
    width:200px;
    margin-top: 20px;
    font-size: 1rem;
    padding:2px
}
.g-media_h:hover{
    text-decoration: underline;
}
.g-check-check{
    margin-top: 40px;
    margin-left:200px;
}
.g-checkable_label{
    margin-left: 220px;
    margin-top: -20px;
    font-size:1rem
}
.p-listControl_edit{
    margin-top:-25px;
    margin-right:500px;
    font-size: 1rem;
}
.g-linkList-lg{
    margin-left: 530px;
    margin-top: -29px;
}
.g-body-wishitem{
    margin-top: 20px;
}
.g-itemList-border, .g-lg-itemList-border {
    border: 0 solid #dbdbdb;
    border-top-width: 1px;
    border-bottom-width: 1px;
}
ul, ol {
    padding: 5px;
}
.g-1234567{
    font-size: 0.8rem;
    margin-top: 20px;
    margin-right: 700px;
}
.g-123456{
    font-size: 0.8rem;
    margin-top: 5px;
    margin-right: 571px;
}
.g-modal_h{
    background-color: #009e96;
    color:white;
    font-size: 1.5rem;
    width:1250px;
    padding:5px
}
.g-modal_close{
    display:flex;
    margin-left:1250px;
    margin-top:-47px;
    height: 45px;
    width:49px
}
.g-modal_body{
    margin-top: 40px;
    margin-bottom: 40px;
    width:1000px;
    text-align: center;
    margin-left:160px
}
</style>
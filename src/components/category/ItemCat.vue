<template>
  <div class="all-sort-list">
    <div
      class="item"
      @mouseover.self="mouseOverFun($event)"
      @mouseleave.self="mouseLeaveFun($event)"
      v-for="(levelOneCat, index1) in categories"
      :key="index1"
    >
      <h3>
        <span>·</span><a href="##">{{ levelOneCat.categoryName }}</a>
      </h3>
      <div class="item-list clearfix" style="top: '0px'; display: none">
        <div class="subitem">
          <dl
            class="fore1"
            v-for="(levelTwoCat, index2) in levelOneCat.categories"
            :key="index2"
          >
            <dt>
              {{ levelTwoCat.categoryName }}
            </dt>
            <dd>
              <em
                v-for="(levelThreeCat, index3) in levelTwoCat.categories"
                :key="index3"
              >
                <a :href="levelThreeCat.link">
                  {{ levelThreeCat.categoryName }}
                </a>
              </em>
            </dd>
          </dl>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { onMounted, computed } from "vue";
// import { mapActions, mapGetters } from "vuex";
import { useStore } from "../../store/index";
const store = useStore();
// const state = reactive({
//   // controll item list show or not
//   displayOrNot: "none",
// });
//let { displayOrNot } = toRefs(state);
const mouseOverFun = (event:MouseEvent) => {
// { target: HTMLInputElement }) => {
  // event.
  let style = 
  "top: " + (event.currentTarget as HTMLElement).offsetTop + "px; display: block";
  (event.currentTarget as HTMLElement)
    .querySelector(".item-list")!
    .setAttribute("style",style);
};

const mouseLeaveFun = (event: MouseEvent) => {
// { target: HTMLInputElement }) => {
  // console.log("event", event);
  let style = "display = none";

  (event.target as HTMLElement)
    .querySelector(".item-list")!
    .setAttribute("style", style);
};


onMounted(() => {
  store.dispatch("setCategories");
});
let categories = computed(() => store.getters.getCategories);
</script>

<style>
.all-sort-list {
  position: relative;
  float: left;
  width: 228px;
  border-top: none;
  background: #000;
  height: 100%;
}
.all-sort-list .item {
  height: 49px;
  border-top: 1px solid #ffffff;
  padding-left: 10px;
}
.all-sort-list .item.bo {
  border-top: none;
}
.all-sort-list .item h3 {
  /* height: 30px;
    line-height: 45px; */
  font-size: 15px;
  font-weight: normal;
  width: 199px;
  overflow: hidden;
  text-align: left;
}
.all-sort-list .item span {
  padding: 0px 5px;
  color: #1baeae;
  font-family: "\5B8B\4F53";
}
.all-sort-list .item a {
  color: #fff;
  text-decoration: none;
}
.all-sort-list .item a:hover {
  font-weight: bold;
  color: #1baeae;
}
.all-sort-list .item-list {
  display: none;
  position: absolute;
  width: 495px;
  min-height: 200px;
  _height: 200px;
  background: #fff;
  left: 210px;
  box-shadow: 0px 0px 10px #dddddd;
  border: 1px solid #ddd;
  top: 3px;
  z-index: 10;
}
.item-list .subitem {
  float: left;
  width: 477px;
  padding: 0px 4px 0px 8px;
}
.item-list .subitem dl {
  border-top: 1px solid #eee;
  padding: 6px 0px;
  overflow: hidden;
  zoom: 1;
}
.item-list .subitem .fore1 {
  border-top: none;
}
.item-list .subitem dt {
  float: left;
  width: 84px;
  line-height: 20px;
  text-align: left;
  padding: 3px 6px 0px 0px;
  font-weight: 700;
  color: #1baeae;
}
.item-list .subitem dt a {
  color: #1baeae;
  text-decoration: underline;
}
.item-list .subitem dd {
  float: left;
  width: 415px;
  padding: 3px 0px 0px;
  overflow: hidden;
}
.item-list .subitem dd em {
  float: left;
  height: 14px;
  line-height: 14px;
  padding: 0px 8px;
  margin-top: 5px;
  border-left: 1px solid #ccc;
}
.item-list .subitem dd em a,
.item-list .cat-right dd a {
  color: #666;
  text-decoration: none;
}
.item-list .subitem dd em a:hover,
.item-list .cat-right dd a:hover {
  font-weight: normal;
  text-decoration: underline;
}
</style>
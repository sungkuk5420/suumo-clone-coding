<template>
  <q-page class="main-page">
    <div class="second-header">
      <q-tabs 
      v-model="mainTab"
      active-color="primary"
      indicator-color="white"
      class="text-dark"
      >
          <q-tab name="home" label="홈" />
          <q-tab name="recommend" label="추천물건" />
          <q-tab name="area" label="지역별" />
          <q-tab name="station" label="역별" />

      </q-tabs>
    </div>
    <div class="main-image">
    </div>
    <div class="mini-category-wrap">
      <MainMiniCategory label="저렴한 월세방"/>
      <MainMiniCategory label="옵션 추천방"/>
      <MainMiniCategory label="인터넷 무료"/>
      <MainMiniCategory label="디자이너 맨션"/>
      <MainMiniCategory label="반려동물 가능"/>
      <MainMiniCategory label="2인 입주 가능  "/>
    </div>
  </q-page>
</template>

<script>
import ComputedMixin from "../ComputedMixin";
import UtilMethodMixin from "../UtilMethodMixin";
import MainMiniCategory from "../components/MainMiniCategory.vue"
export default {
  mixins: [ComputedMixin, UtilMethodMixin],
  components: {
    MainMiniCategory
  },
  data () {
    return {
      mainTab: "home"
    };
  },
  mounted() {
    // this.showLoading();
  },
};
</script>

<style lang="scss">
.main-page{
  display: flex;
  flex-direction: column;
  width: 100%;
  overflow: auto;
  padding-bottom: 50px;

  .second-header{
      height: 65px;
      margin-left: auto;
      margin-right: auto;
      .q-tabs{
          display: flex;
          margin-right: auto;
          width: 300px;
      }
  }
  .main-image{
    display: table;
    min-width:1420px;
    min-height: 635px;
    background: #e6e6e6;
  }
  .mini-category-wrap{
    margin-top: 50px;
    display: flex;
    justify-content: space-around;
  }
}
</style>

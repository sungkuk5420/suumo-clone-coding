<template>
  <q-page class="main-page">
    <img alt="Quasar logo" src="~assets/quasar-logo-full.svg" />
  </q-page>
</template>

<script>
import ComputedMixin from "../ComputedMixin";
import UtilMethodMixin from "../UtilMethodMixin";
export default {
  mixins: [ComputedMixin, UtilMethodMixin],
  mounted() {
    // this.showLoading();
  },
};
</script>

<style>
.main-page{
  display: flex;

}
</style>

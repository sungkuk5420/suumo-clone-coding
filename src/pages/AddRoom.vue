<template>
  <div class="flex flex-center add-room">
    addroom
  </div>
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
.add-room{
  
}
</style>

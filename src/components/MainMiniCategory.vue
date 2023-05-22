<template>
  <div class="main-mini-category" >
    <div class="main-mini-category__image"></div>
    <div class="main-mini-category__label">{{ label }}</div>
  </div>
</template>

<script>
export default {
  name: "EssentialLink",
  props: ["label"],
};
</script>

<style lang="scss">
  .main-mini-category{
    cursor: pointer;
    &__image{
      width: 100px;
      height: 100px;
      background: #e6e6e6;
      border-radius: 30px;
    }
    &__label{
      text-align: center;
      margin-top: 10px;
      font-size: 18px;
    }
  }
</style>

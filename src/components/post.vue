<script setup>
const props = defineProps({
  title:String,
  body: String,
  userName: String
})
</script>

<template>
  <div class="post">
    <h3 class="post__title">{{ title }}</h3>
    <p class="post__text">{{ body }}</p>
    <p class="post__author">{{ userName }}</p>
  </div>
</template>

<style scoped lang="scss">
.post{
  background: #fff;
  padding: 20px;
  border-radius: 8px;
  border: 1px solid #ced2d5;
  display: flex;
  flex-direction: column;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05);
  &__title{
    color: #4a8fe3;
    font-size: 20px;
    line-height: 120%;
    margin:0 0 15px;
  }
  &__text{
    color: #4e5156;
    font-size: 18px;
    line-height: 120%;
    margin:0 0 auto;
  }
  &__author{
    color: #ced2d5;
    font-size: 16px;
    line-height: 120%;
    margin: 15px 0 0;
  }
}
</style>
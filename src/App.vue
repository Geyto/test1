<script setup>
import Post from "@/components/post.vue";
import axios from "axios";
import {computed, onMounted, ref} from "vue";
const posts  = ref([])
const users  = ref([])
const searchQuery = ref('')
const getPosts = async () => {
  try {
    const {data} = await axios.get(`https://jsonplaceholder.typicode.com/posts`);
    posts.value = data
  } catch (e) {
    console.log(e)
  }
}
const getUsers = async () => {
  try {
    const {data} = await axios.get(`https://jsonplaceholder.typicode.com/users`);
    users.value = data
  } catch (e) {
    console.log(e)
  }
}

const result = computed(() => {
  return posts.value
      .map((item1) => {
    const findName = users.value.find((item2) => item2.id === item1.userId);
    return {
      ...item1,
      userName: findName ? findName.name : null
    };
  })
      .filter(post => {
        return post.userName?.toLowerCase().includes(searchQuery.value.toLowerCase());
      });
});
onMounted(() => {
  getPosts();
  getUsers();
});
</script>

<template>
  <div class="main">
    <div class="main__container">
<!--      <Filter/>-->
      <form class="main__search" id="search">
        <label class="main__search-label">
          <img class="main__search-img" src="./assets/img/search.svg" alt=""><input class="main__search-input" placeholder="Сортировка по автору..." name="query" v-model="searchQuery">
        </label>

      </form>
      <div class="main__posts" v-auto-animate v-if="result.length !== 0">
        <Post v-for="post in result"
              :key="post.id"
              :title="post.title"
              :body="post.body"
              :userName="post.userName"
              :filter-key="searchQuery"
        />
      </div>
      <div class="main__empty" v-else>
        <p class="main__empty-text">По данному запросу ничего не нашлось</p>
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
[class*="__container"] {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 16px;
}
.main{
  &__posts{
    display: grid;
    gap: 25px;
    grid-template-columns: repeat(auto-fill , minmax(300px, 1fr ));
  }
  &__search{
    display: flex;
    align-items: center;
    justify-content: center;
    margin-bottom: 30px;
    &-label{
      display: flex;
      align-items: center;
      justify-content: center;
      max-width: 400px;
      width: 100%;
    }
    &-input{
      padding: 8px;
      background: white;
      color: #4e5156;
      font-size: 18px;
      line-height: 120%;
      border: 1px solid #ced2d5;
      outline:none;
      border-radius: 0 8px 8px 0;
      &::placeholder{
        color: #ced2d5;
      }
    }
    img{
      max-width: 22px;
      background: white;
      border: 1px solid #ced2d5;
      border-right: none;
      border-radius:  8px 0 0 8px ;
      height: 100%;
      padding: 8px;
      cursor: pointer;
    }
  }
  &__empty-text{
    color: #4e5156;
    font-size: 24px;
    line-height: 120%;
  }
}


</style>

<template>
  <div class="navbar">
    <div @click="$router.push('/')" class="logo">vue3</div>
    <div class="navbar__btns">
        <my-button @click="$router.push('/posts')">posts</my-button>
        <my-button @click="$router.push('/store')">posts out of store</my-button>
        <my-button @click="$router.push('/comp')">posts out of composition</my-button>
        <my-button @click="$router.push('/about')">about</my-button>
        <my-button @click="$router.push('/')">home</my-button>
    </div>
  </div>
</template>

<script>
import MyButton from './UI/MyButton.vue'
export default {
  components: { MyButton },

}
</script>

<style scoped>
.navbar {
    min-height: 50px;
    background: transparent;
    box-shadow: 2px 2px 4px blue;
    display: flex;
    align-items: center;
    padding: 10px;
    justify-content: space-around;
}
.navbar .navbar__btns {
    display: flex;
    justify-content: space-between;
    cursor: pointer;
}

.logo {
    cursor: pointer;
    font-size: 2rem;
    color: blue;
}
</style>
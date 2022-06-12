<template >
  <div v-if="posts.length > 0">
    <h3>List of Items</h3>
    <transition-group name="post-list">
      <post-item
        v-for="post of posts"
        :key="post.title"
        :post="post"
        @remove="$emit('remove', post)"
      />
    </transition-group>
  </div>
  <h2 v-else>No posts yet..</h2>
</template>
<script>
import PostItem from "./PostItem.vue";
export default {
  components: {
    PostItem,
  },
  props: {
    posts: {
      type: Array,
      required: true,
    },
  },
};
</script>
<style scoped>

  .post-list-enter-active,
  .post-list-leave-active {
    transition: all .5s ease;
  }
  .post-list-enter-from,
  .post-list-leave-to {
    opacity: 0;
    transform: translateY(-30px);
  }
  .post-list-move {
    transition: all .5s ease;
  }
</style>
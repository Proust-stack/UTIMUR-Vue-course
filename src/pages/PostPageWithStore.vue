<template>
  <div>
    <h1>post page</h1>
    <my-input
      :model-value="searchQuery"
      @update:model-value="setSearchQuery"
      placeholder="search..."
      class="input__search"
      v-focus
    />
    <div class="app__btns">
      <my-button @click="showDialog">create post</my-button>
      <my-select 
      :model-value="selectedSort" 
      @update:model-value="setSelectedSort"
      :options="sortOptions" 
      />
    </div>
    <my-dialog v-model:show="dialogVisible">
      <post-form @create="createPost" />
    </my-dialog>
    <div class="page__wrapper">
      <div
        v-for="pageNumber in totalPages"
        :key="pageNumber"
        class="page"
        :class="{
          'current-page': page === pageNumber,
        }"
        @click="changePage(pageNumber)"
      >
        {{ pageNumber }}
      </div>
    </div>
    <post-list
      :posts="sortedAndSearchedPosts"
      @remove="removePost"
      v-if="!isPostsLoading"
    />
    <div v-else>loading</div>
    <div v-intersection="fetchMorePosts" class="observer"></div>
  </div>
</template>

<script>
import PostList from "@/components/PostList.vue";
import PostForm from "@/components/PostForm.vue";
import { mapState, mapGetters, mapActions, mapMutations } from "vuex";

export default {
  components: {
    PostList,
    PostForm,
  },
  data() {
    return {
      dialogVisible: false,
    };
  },
  methods: {
    ...mapActions({
      fetchMorePosts: "post/fetchMorePosts",
      fetchPosts: "post/fetchPosts",
    }),
    ...mapMutations({ 
      setPage: "post/setPage",
      setSearchQuery: 'post/setSearchQuery',
      setSelectedSort: 'post/setSelectedSort'
      }),
    changePage(pageNumber) {
      this.page = pageNumber;
    },
    createPost(post) {
      const newPost = {
        id: this.posts.length + 1,
        title: post.title,
        body: post.body,
      };
      this.posts.unshift(newPost);
      this.dialogVisible = false;
    },
    removePost(post) {
      this.posts = this.posts.filter((p) => p.id !== post.id);
    },
    showDialog() {
      this.dialogVisible = true;
    },
  },
  mounted() {
    this.fetchPosts();
  },
  computed: {
    ...mapState({
      posts: state => state.post.posts,
      isPostsLoading: state => state.post.isPostsLoading,
      selectedSort: state => state.post.selectedSort,
      searchQuery: state => state.post.searchQuery,
      page: state => state.post.page,
      limit: state => state.post.limit,
      totalPages: state => state.post.totalPages,
      sortOptions: state => state.post.sortOptions,
    }),
    ...mapGetters({
      sortedPosts: "post/sortedPosts",
      sortedAndSearchedPosts: "post/sortedAndSearchedPosts",
    }),
  },
  watch: {
    // page() {
    //   this.fetchPosts();
    // },
  },
};
</script>

<style>
h1 {
  text-align: center;
  margin-top: 30px;
}

.app__btns {
  margin: 15px 0;
  display: flex;
  justify-content: space-between;
}

.page__wrapper {
  display: flex;
  margin: 30px 0;
}
.page__wrapper .page {
  display: flex;
  border: 1px solid black;
  padding: 10px;
  margin-right: 5px;
  cursor: pointer;
}
.page__wrapper .current-page {
  background: #000;
  color: white;
}

.input__search {
  margin: 15px 0;
}

.observer {
  height: 30px;
  background: green;
}
</style>
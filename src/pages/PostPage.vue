<template>
  <div>
    <h1>post page</h1>
    <my-input
      v-model="searchQuery"
      placeholder="search..."
      class="input__search"
      v-focus
    />
    <div class="app__btns">
      <my-button @click="showDialog">create post</my-button>
      <my-select v-model="selectedSort" :options="sortOptions" />
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
import axios from "axios";

import PostList from "@/components/PostList.vue";
import PostForm from "@/components/PostForm.vue";
export default {
  components: {
    PostList,
    PostForm,
  },
  data() {
    return {
      posts: [],
      dialogVisible: false,
      isPostsLoading: false,
      selectedSort: "",
      searchQuery: "",
      page: 1,
      limit: 10,
      totalPages: 0,
      sortOptions: [
        { value: "title", name: "By title" },
        { value: "body", name: "By description" },
      ],
    };
  },
  methods: {
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
    async fetchPosts() {
      try {
        this.isPostsLoading = true;
        const response = await axios.get(
          "https://jsonplaceholder.typicode.com/posts",
          {
            params: {
              _page: this.page,
              _limit: this.limit,
            },
          }
        );
        this.totalPages = Math.ceil(
          response.headers["x-total-count"] / this.limit
        );
        this.posts = response.data;
      } catch (error) {
        console.log(error);
      } finally {
        this.isPostsLoading = false;
      }
    },
    async fetchMorePosts() {
      try {
        this.page += 1;
        const response = await axios.get(
          "https://jsonplaceholder.typicode.com/posts",
          {
            params: {
              _page: this.page,
              _limit: this.limit,
            },
          }
        );
        this.totalPages = Math.ceil(
          response.headers["x-total-count"] / this.limit
        );
        this.posts = [...this.posts, ...response.data];
      } catch (error) {
        console.log(error);
      } 
    },
  },
  mounted() {
    this.fetchPosts();
  },
  computed: {
    sortedPosts() {
      return [...this.posts].sort((post1, post2) =>
        post1[this.selectedSort]?.localCompare(post2[this.selectedSort])
      );
    },
    sortedAndSearchedPosts() {
      return this.sortedPosts.filter((post) => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()));
    },
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
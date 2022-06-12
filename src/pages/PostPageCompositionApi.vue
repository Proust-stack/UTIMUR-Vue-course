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
      <my-select v-model="selectedSort" :options="sortOptions" />
    </div>
    <my-dialog v-model:show="dialogVisible">
      <post-form />
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
      v-if="!isPostsLoading"
    />
    <div v-else>loading</div>
  </div>
</template>

<script>
import PostList from "@/components/PostList.vue";
import PostForm from "@/components/PostForm.vue";
import { usePosts } from "@/hooks/usePosts";
import { useSortedPosts } from "@/hooks/useSortedPosts";
import useSortedAndSearchedPosts from "@/hooks/useSortedAndSearchedPosts";
export default {
  components: {
    PostList,
    PostForm,
  },
  data() {
    return {
      dialogVisible: false,
      sortOptions: [
        { value: "title", name: "By title" },
        { value: "body", name: "By description" },
      ],
    };
  },
  setup(props) {
    const { posts, totalPage, isPostsLoading } = usePosts(10);
    const { selectedSort, sortedPosts } = useSortedPosts(posts);
    const { sortedAndSearchedPosts, searchQuery } = useSortedAndSearchedPosts(sortedPosts);
    return {
      posts,
      totalPage,
      isPostsLoading,
      sortedPosts,
      selectedSort,
      searchQuery,
      sortedAndSearchedPosts,
    };
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
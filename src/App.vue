<template>
  <div style="margin: 30px">
    <custom-input v-model="searchQuery" />
    <div class="app__btns">
      <custom-button @click="showMoadal"> Create Post </custom-button>
      <custom-select
        v-model="selectedSort"
        :options="sortOptions"
      ></custom-select>
    </div>

    <custom-modal v-model:show="dialogVisible">
      <post-form @create="createPost" />
    </custom-modal>

    <post-list
      v-if="!isPostsLoading"
      @remove="removePost"
      :posts="sortedAndSearchedPost"
    />
    <div v-else>Is loading...</div>
    <!-- <div class="page__wrapper">
      <div
        v-for="pageNumber in totalPage"
        :key="pageNumber"
        class="page"
        :class="{ current_page: page === pageNumber }"
        @click="changePage(pageNumber)"
      >
        {{ pageNumber }}
      </div>
    </div> -->
  </div>
</template>

<script>
import axios from "axios";
import PostForm from "./components/PostForm.vue";
import PostList from "./components/PostList.vue";
export default {
  components: {
    PostForm,
    PostList,
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
      totalPage: 0,
      sortOptions: [
        { value: "title", name: "by title" },
        { value: "body", name: "by body" },
      ],
    };
  },
  methods: {
    createPost(post) {
      this.posts.push(post);
      this.dialogVisible = false;
    },
    removePost(post) {
      this.posts = this.posts.filter((item) => item.id !== post.id);
    },
    showMoadal() {
      this.dialogVisible = true;
    },
    // changePage(pageNumber) {
    //   this.page = pageNumber;
    // },
    async fetchPosts() {
      try {
        this.isPostsLoading = true;
        const respone = await axios.get(
          "https://jsonplaceholder.typicode.com/posts",
          {
            params: {
              _page: this.page,
              _limit: this.limit,
            },
          }
        );
        this.totalPage = Math.ceil(
          respone.headers["x-total-count"] / this.limit
        );
        this.posts = respone.data;
      } catch (error) {
        alert("error : ", error);
      } finally {
        this.isPostsLoading = false;
      }
    },
  },
  mounted() {
    this.fetchPosts();
  },
  computed: {
    sortedPost() {
      return [...this.posts].sort((post_first, post_second) =>
        post_first[this.selectedSort]?.localeCompare(
          post_second[this.selectedSort]
        )
      );
    },
    sortedAndSearchedPost() {
      return this.sortedPost.filter((post) =>
        post.title.toLowerCase().includes(this.searchQuery.toLowerCase())
      );
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
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
.app__btns {
  display: flex;
  justify-content: space-between;
  margin: 20px 0px;
}

.page__wrapper {
  display: flex;
  margin-top: 15px;
}
.page {
  border: 1px solid black;
  padding: 10px;
}
.current_page {
  border: 2px solid green;
}
</style>

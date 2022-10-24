<template>
  <div class="wrapper">
    <h1>Posts Page</h1>
    <my-input v-focus :model-value="searchQuery" @update:model-value="setSearchQuery" placeholder="search" />
    <div class="app_btn">
      <my-button @click="showDialog">Add new post</my-button>
      <my-select :model-value="selectedSort" @update:model-value="setSelectedSort" :options="sortOptions"></my-select>
    </div>
    <my-dialog v-model:show="dialogVisible">
      <post-form @create="createPost" />
    </my-dialog>
    <div class="page_wrapper">
      <div v-for="pageNumber in totalPages" :key="pageNumber" class="page" :class="{
        'current-page': page === pageNumber
      }" @click="changePage(pageNumber)">{{ pageNumber }}</div>
    </div>
    <post-list :posts="sortedAndSearchedPosts" @remove="removePost" v-if="!isPostsLoading" />
    <div v-else>loading...</div>
    <div v-intersection="loadMorePosts"></div>
  </div>
</template>

<script>
import PostForm from "@/components/PostForm";
import PostList from "@/components/PostList";
import { mapState, mapGetters, mapActions, mapMutations } from "vuex";
export default {
  data() {
    return {
      dialogVisible: false,
    };
  },
  methods: {
    ...mapMutations({
      setPage: 'post/setPage',
      setSearchQuery: 'post/setSearchQuery',
      setSelectedSort: 'post/setSelectedSort',
    }),
    ...mapActions({
      loadMorePosts: 'post/loadMorePosts',
      fetchPosts: 'post/fetchPosts'
    }),
    createPost(post) {
      this.posts.push(post)
      this.dialogVisible = false
    },
    removePost(post) {
      this.posts = this.posts.filter(p => p.id !== post.id)
    },
    showDialog() {
      this.dialogVisible = true
    },
  },
  components: { PostForm, PostList },
  mounted() {
    this.fetchPosts()

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
      sortedPosts: 'post/sortedPosts',
      sortedAndSearchedPosts: 'post/sortedAndSearchedPosts',
    }),
  },
  watch: {
    // page() {
    //   this.fetchPosts()
    // }
  },
}
</script>

<style>
.wrapper {
  margin-top: 50px;
}

.app_btn {
  display: flex;
  justify-content: space-between;
  margin: 20px 0;
}

.page_wrapper {
  display: flex;
  margin: 15px 0;
}

.page {
  border: 2px solid teal;
  padding: 10px;
  cursor: pointer;
  margin: 0 5px;
}

.current-page {
  background-color: teal;
}

.observer {
  height: 30px;
  background-color: green;
}
</style>
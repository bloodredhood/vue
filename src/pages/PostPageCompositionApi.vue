<template>
  <div class="wrapper">
    <h1>Posts Page</h1>
    <my-input v-focus v-model="searchQuery" placeholder="search" />
    <div class="app_btn">
      <my-button @click="showDialog">Add new post</my-button>
      <my-select v-model="selectedSort" :options="sortOptions"></my-select>
    </div>
    <my-dialog v-model:show="dialogVisible">
      <post-form @create="createPost" />
    </my-dialog>
    <post-list :posts="sortedAndSearchedPosts" @remove="removePost" v-if="!isPostsLoading" />
    <div v-else>loading...</div>
    <div v-intersection="loadMorePosts" ></div>
  </div>
</template>

<script>
import PostForm from "@/components/PostForm";
import PostList from "@/components/PostList";
import axios from "axios";
import {usePosts} from "@/hooks/usePosts";
import {useSortedPosts} from "@/hooks/useSortedPosts";
import {useSortedAndSearchedPosts} from "@/hooks/useSortedAndSearchedPosts";
export default {
  data() {
    return {
      dialogVisible: false,
      page: 1,
      sortOptions: [
        { value: "title", name: "by name" },
        { value: "body", name: "by innertext" },
        { value: "id?.", name: "by id" },
      ]
    };
  },
  setup(props) {
    const {posts, totalPages, isPostsLoading} = usePosts(10, 1)
    const {selectedSort, sortedPosts} = useSortedPosts(posts)
    const { searchQuery, sortedAndSearchedPosts} = useSortedAndSearchedPosts(sortedPosts)

    return {
      posts, totalPages, isPostsLoading, selectedSort, sortedPosts, searchQuery, sortedAndSearchedPosts
    }
  },
  components: { PostForm, PostList },
  methods: {
    async loadMorePosts() {
      try {
        this.page += 1
        const response = await axios.get("https://jsonplaceholder.typicode.com/posts", {
          params: {
            _page: this.page,
            _limit: this.limit,
          }
        })
        this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
        this.posts = [...this.posts, ...response.data]
      } catch (e) {
        console.log(e)
      } finally {
      }
    }
  }
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
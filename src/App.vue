<template>
  <div class="app">
    <h1>Posts Page</h1>
    <my-input v-model="searchQuery" placeholder="search" />
    <div class="app_btn">
      <my-button @click="showDialog">Add new post</my-button>
      <my-select v-model="selectedSort"
      :options="sortOptions"
      ></my-select>
    </div>
    <my-dialog v-model:show="dialogVisible">
      <post-form @create="createPost"/>
    </my-dialog>
    <div class="page_wrapper">
      <div
      v-for="pageNumber in totalPages"
      :key="pageNumber"
      class="page"
      :class="{
        'current-page': page === pageNumber
        }"
        @click="changePage(pageNumber)"
      >{{pageNumber}}</div>
    </div>
    <post-list :posts="sortedAndSearchedPosts" @remove="removePost"
    v-if="!isPostLoading"/>
    <div v-else>loading...</div>
  </div>
</template>

<script>
import PostForm from "@/components/PostForm";
import PostList from "@/components/PostList";
import axios from "axios"
export default {
  data() {
    return {
      posts: [],
      dialogVisible: false,
      isPostLoading: false,
      selectedSort: "",
      searchQuery: "",
      page: 1,
      limit: 10,
      totalPages: 0,
      sortOptions: [
        {value: "title", name: "by name"},
        {value: "body", name: "by innertext"},
      ]
    };
  },
  methods: {
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
    changePage(pageNumber) {
      this.page = pageNumber
    },
    async fetchPosts() {
      try {
        this.isPostLoading = true
        const response = await axios.get("https://jsonplaceholder.typicode.com/posts", {
          params: {
            _page: this.page,
            _limit: this.limit,
          }
        })
        this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
        this.posts = response.data
        console.log(response.data)
      } catch {
        alert("Error!")
      } finally {
        this.isPostLoading = false
      }
    }
  },
  components: { PostForm, PostList },
  mounted() {
    this.fetchPosts()
  },
  computed: {
    sortedPosts() {
      return [...this.posts].sort((post1, post2) => post1[this.selectedSort]?.localeCompare(post2[this.selectedSort]))
    },
    sortedAndSearchedPosts() {
      return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
    }
  },
  watch: {
    page() {
      this.fetchPosts()
    }
  },
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.app {
  padding: 20px;
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
</style>
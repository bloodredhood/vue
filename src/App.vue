<template>
  <div class="app">
    <h1>Posts Page</h1>
    <div class="app_btn">
      <my-button @click="showDialog">Add new post</my-button>
      <my-select v-model="selectedSort"
      :options="sortOptions"
      ></my-select>
    </div>
    <my-dialog v-model:show="dialogVisible">
      <post-form @create="createPost"/>
    </my-dialog>
    <post-list :posts="posts" @remove="removePost"
    v-if="!isPostLoading"/>
    <div v-else>loading...</div>
  </div>
</template>

<script>
import PostForm from "@/components/PostForm";
import PostList from "@/components/PostList";
import axios from "axios"
import MyButton from "./components/UI/MyButton.vue";
import MySelect from "./components/UI/MySelect.vue";
export default {
  data() {
    return {
      posts: [],
      dialogVisible: false,
      isPostLoading: false,
      selectedSort: "",
      sortOptions: [
        {value: "title", name: "by name"},
        {value: "body", name: "by innertext"},
        {value: "id", name: "by id"},
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
    async fetchPosts() {
      try {
        this.isPostLoading = true
        const response = await axios.get("https://jsonplaceholder.typicode.com/posts?_limit=10")
        this.posts = response.data
      } catch {
        alert("Error!")
      } finally {
        this.isPostLoading = false
      }
    }
  },
  components: { PostForm, PostList, MyButton, MySelect },
  mounted() {
    this.fetchPosts()
  }
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
</style>
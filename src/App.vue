<template>
  <div class="app">
    <h1>Posts Page</h1>
    <my-button @click="fetchPosts">get posts</my-button>
    <my-button style="margin: 20px 0;" @click="showDialog">Add new post</my-button>
    <my-dialog v-model:show="dialogVisible">
      <post-form @create="createPost"/>
    </my-dialog>
    <post-list :posts="posts" @remove="removePost"/>
  </div>
</template>

<script>
import PostForm from "@/components/PostForm";
import PostList from "@/components/PostList";
import axios from "axios"
import MyButton from "./components/UI/MyButton.vue";
export default {
  data() {
    return {
      posts: [
        { id: 1, title: "JS", body: "descr of JS" },
        { id: 2, title: "python", body: "descr of python" },
        { id: 3, title: "C", body: "descr of C" },
      ],
      dialogVisible: false,
      modifictorValue: "",
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
        const response = await axios.get("https://jsonplaceholder.typicode.com/posts?_limit=10")
        console.log(response)
      } catch {
        alert("Error!")
      }
    }
  },
  components: { PostForm, PostList, MyButton }
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
</style>
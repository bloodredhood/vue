<template>
  <div class="app">
    <post-form @create="createPost"/>
    <post-list :posts="posts" />
  </div>
</template>

<script>
import PostForm from "@/components/PostForm";
import PostList from "@/components/PostList";
export default {
  data() {
    return {
      posts: [
        { id: 1, title: "JS", body: "descr of JS" },
        { id: 2, title: "python", body: "descr of python" },
        { id: 3, title: "C", body: "descr of C" },
      ],
    };
  },
  methods: {
    createPost(post) {
      this.posts.push(post)
    },
  },
  components: { PostForm, PostList }
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
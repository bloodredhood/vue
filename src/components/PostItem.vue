<template>
  <div class="post">
    <div>
      <div><strong>{{post.id}}</strong></div>
      <div><strong>Name: </strong>{{post.title}}</div>
      <div><strong>Description: </strong>{{post.body}}</div>
    </div>
    <my-button class="post_bnts" @click="$router.push(`/posts/${post.id}`)" >Open post
    </my-button>
    <my-button @click="$emit('remove', post)" >Delete post
    </my-button>
  </div>
</template>

<script>
export default {
  props: {
    post: {
      type: Object,
      required: true,
    }
  },
}
</script>

<style scoped>
.post {
  padding: 15px;
  border: 2px solid teal;
  margin-top: 15px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.post_btns {
  display: flex;
}
</style>
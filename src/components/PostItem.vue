<template>
  <div class="post">
    <div>
      <div><strong>Name: </strong>{{post.title}}</div>
      <div><strong>Description: </strong>{{post.body}}</div>
    </div>
    <my-button>Delete post</my-button>
  </div>
</template>

<script>
import MyButton from '@/components/UI/MyButton';

export default {
  props: {
    post: {
      type: Object,
      required: true,
    }
  },
  components: { MyButton }
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
</style>
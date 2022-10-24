<template>
  <div class="wrapper">
    <h1>
      Welcome to our App
    </h1>
    <div>
      <router-link to="/posts">to Posts...</router-link>
    </div>
    <div>
      <router-link to="/about">about App</router-link>
    </div>
  </div>
</template>

<script>
export default {

}
</script>

<style scoped>
.wrapper {
  margin-top: 50px;
}
</style>
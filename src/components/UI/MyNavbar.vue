<template>
  <div class="navbar">
    <div @click="$router.push('/')" style="cursor: pointer">Vue 3</div>
    <div class="navbar_btns">
      <my-button @click="$router.push('/posts')">Posts</my-button>
      <my-button @click="$router.push('/about')" style="margin-left: 20px">About</my-button>
      <my-button @click="$router.push('/store')" style="margin-left: 20px">store</my-button>
      <my-button @click="$router.push('/composition')" style="margin-left: 20px">composition</my-button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'my-navbar',
}
</script>

<style>
.navbar {
  position: fixed;
  width: 100%;
  height: 50px;
  background-color: lightgray;
  box-shadow: 2px 2px 4px gray;
  display: flex;
  align-items: center;
  padding: 0 15px;
}

.navbar_btns {
  margin-left: auto;
}
</style>
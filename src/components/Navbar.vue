<template>
  <div class="navbar">
    <div @click="$router.push('/')">Vue</div>
    <div class="navbar__btns">
      <my-button @click="$router.push('/posts')">Посты</my-button>
      <my-button @click="$router.push('/about')" style="margin-left: 10px"
        >О сайте</my-button
      >
      <my-button @click="$router.push('/store')" style="margin-left: 10px"
        >store</my-button
      >
      <my-button @click="$router.push('/composition')" style="margin-left: 10px"
        >composition</my-button
      >
    </div>
  </div>
</template>

<script></script>

<style scoped>
.navbar {
  height: 50px;
  background-color: teal;
  box-shadow: 2px 2px 4px gray;
  display: flex;
  align-items: center;
  padding: 0 15px;
}
.navbar__btns {
  margin-left: auto;
}
</style>

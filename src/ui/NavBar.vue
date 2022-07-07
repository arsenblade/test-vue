<template>
  <div class="navbar">
    <h1>Vue 3</h1>
    <div class="navbar__btns">
      <my-button @click="$router.push('/')">Главная</my-button>
      <my-button @click="$router.push('/posts')">Посты</my-button>
    </div>
  </div>
</template>
<script>
export default {
  
}
</script>
<style>
.navbar {
  height: 78px;
  background-color: rgba(56, 250, 160, 0.592);
  box-shadow: 2px 2px 4px gray;
  display: flex;
  justify-content: space-between;
  padding: 20px
}

.navbar__btns {
  display: flex;
  column-gap: 10px;
}
</style>
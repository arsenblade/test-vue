<template>
  <div v-if="posts.length > 0">
  <transition-group name="posts-list">
    <post-item v-for="post in posts" :post="post" :key="post.id" @remove="$emit('remove', post)"/>
  </transition-group>
  </div>
  <h1 v-else style="color: red">Нет постов</h1>
</template>
<script>
import PostItem from './PostItem.vue';
export default {
    props: {
        posts: {
            type: Array,
            required: true,
        }
    },
    components: { PostItem }
}
</script>
<style>
.posts-list-item {
  display: inline-block;
  margin-right: 10px;
}
.posts-list-enter-active,
.posts-list-leave-active {
  transition: all 0.4s ease;
}
.posts-list-enter-from,
.posts-list-leave-to {
  opacity: 0;
  transform: translateX(130px);
}

.posts-list-move {
  transition: transform 0.8s ease;
}
</style>
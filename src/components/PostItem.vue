<template>
  <div class="post">
    <div>
      <h5>{{post.id}}</h5>
      <div><strong>Название:</strong>{{post.title}}</div>
      <div><strong>Описание:</strong>{{post.body}}</div>
    </div>
    <div class="post-btns">
      <my-button
        @click="$router.push(`/posts/${post.id}`)"
      >Открыть</my-button>
      <my-button
      @click="$emit('remove', post)"
      >Удалить</my-button>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    post: {
      type: Object,
      required: true,
    }
  }
}
</script>
<style>
 .post {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 15px;
  padding: 15px;
  border: 2px solid green;
  border-radius: 6px;
} 

.post-btns {
  display: flex;
  column-gap: 10px;
}
</style>
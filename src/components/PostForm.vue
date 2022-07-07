<template>
<form @submit.prevent>
      <h4>Создание поста</h4>
      <my-input v-model="post.title" placeholder="Название" />
      <my-input v-model="post.body" placeholder="Описание" />
      <my-button class="btn-create" @click="createPost">Создать</my-button>
    </form>
</template>
<script>
export default {
  data() {
    return {
      post: {
        body: "",
        title: ""
      }
    }
  },
  methods: {
    createPost() {
      this.post.id = Date.now()
      this.$emit('create', this.post)
      this.post = {
        title: "",
        body: ""
      }
    }
  },
}
</script>
<style>
form {
  display: flex;
  flex-direction: column;
}

.btn-create {
  align-self: flex-end;
}

.btn-create:hover {
  background-color: #00FF99;
}
</style>
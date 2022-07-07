<template>
  <div>
    <h1>Страница с постами</h1>
    <my-input 
    v-model="searchQuery"
    />
    <div class="app__buttons">
      <my-button @click="showDialog">Создать пользователя</my-button>
      <my-select v-model="selectedSort" :options="sortOptions" />
    </div>
    <my-dialog v-model:show="dialogVisible">
      <post-form 
        @create="createPost"
      />
    </my-dialog>
    <post-list :posts="sortedAndSearchedPosts" @remove="removePost" v-if="isLoadingPosts === false"/>
    <div v-else>Идет загрузка...</div>
    <div ref="observer" class="observer"></div>
  </div>
</template>
<script>
import PostForm from '@/components/PostForm'
import PostList from '@/components/PostList'
import axios from 'axios'
export default {
  components: {
    PostList, PostForm
  },
  data() {
    return {
      posts: [],
      dialogVisible: false,
      isLoadingPosts: false,
      selectedSort: '',
      searchQuery: '',
      page: 1,
      limit: 10,
      totalPages: 0,
      sortOptions: [
        {value: 'title', name: 'По названию'},
        {value: 'body', name: 'По описанию'},
      ]
    }
  },
  methods: {
    createPost(post) {
      this.posts.push(post)
      this.dialogVisible = false
    },
    removePost(post) {
      this.posts = this.posts.filter(p => p.id !== post.id)
    },
    showDialog() {
      this.dialogVisible = true
    },
    async fetchPosts() {
      try {
        this.isLoadingPosts = true
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
          params: {
            _limit: this.limit,
            _page: this.page
          }
        })
        this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)

        this.posts = response.data
      } catch (e) {
        console.log(e)
      } finally {
        this.isLoadingPosts = false
      }
    },
    async loadMorePosts() {
      try {
        this.page += 1
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
          params: {
            _limit: this.limit,
            _page: this.page
          }
        })
        this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)

        this.posts = [...this.posts, ...response.data]
      } catch (e) {
        console.log(e)
      } finally {
        this.isLoadingPosts = false
      }
    }
  },
  mounted() {
    this.fetchPosts()
    const options = {
    rootMargin: '0px',
    threshold: 1.0
    }
    const callback = (entries, observer) => {
        if(entries[0].isIntersecting && this.page < this.totalPages) {
          this.loadMorePosts()
        }
    };
    const observer = new IntersectionObserver(callback, options);
    observer.observe(this.$refs.observer)
  },
  computed: {
    sortedPosts() {
      return [...this.posts].sort((post1, post2) => post1[this.selectedSort]?.localeCompare(post2[this.selectedSort]))
    },
    sortedAndSearchedPosts() {
      return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
    }

  }
}
</script>
<style>
.app__buttons {
  display: flex;
  justify-content: space-between;
  margin-top: 10px;
}

.page__wrapper {
  display: flex;
  margin-top: 15px;
  column-gap: 4px;
}

.page {
  border: 1px solid green;
  padding: 10px;
  border-radius: 2px;
  cursor: pointer;
}

.page:hover {
  background-color: #00FF99;
}

.page:active {
  background-color: #1fca86;
}

.current-page {
  background-color: #00b76e;
  border: 2px solid green;
}
</style>
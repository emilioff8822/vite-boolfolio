
<script>
import { store } from './store/store.js';
import axios from 'axios';

export default {
  name: 'home',
  data() {
    return {
      posts: [],
      links: [],
      first_page_url: null,
      last_page_url: null,
      current_page: null,
      last_page: null
    };
  },
  methods: {
    getApi(endpoint) {
      console.log(store.apiUrl);
      //faccio la chiamata axios
      axios.get(endpoint)
        .then(results => {
          this.posts = results.data.data
          this.links = results.data.links
          this.first_page_url = results.data.first_page_url
          this.last_page_url = results.data.last_page_url
          this.current_page = results.data.current_page
          this.last_page = results.data.last_page
        });
    },
    formatData(dateString) {
      const d = new Date(dateString);
      return d.toLocaleDateString();
    }
  },
  mounted() {
    this.getApi(store.apiUrl + 'posts');
  }
}
</script>

<template>
  <div class="container">
    <h1>Elenco Post</h1>

    <div class="card-container">
      <div class="card" v-for="post in posts" :key="post.id">
        <h2 class="card-title">Titolo: {{ post.title }}</h2>
        <p class="card-date">Data: {{ formatData(post.date) }}</p>
        <p class="card-category">Categoria: <strong>{{ post.category.name }}</strong></p>
        <div v-for="tag in post.tags" :key="tag.id" class="card-tags">
          Tag: <strong>{{ tag.name }}</strong>
        </div>
      </div>
    </div>

    <div class="pagination">
      <button @click="getApi(first_page_url)" :disabled="current_page == 1">Inizio</button>
      <button v-for="(link, index) in links" :key="index" v-html="link.label" @click="getApi(link.url)"
        :disabled="link.active || !link.url"></button>
      <button @click="getApi(last_page_url)" :disabled="current_page == last_page">Fine</button>
    </div>

  </div>
</template>



<style lang="scss">

@use './scss/style.scss';




</style>


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

    <div class="row">
      <div class="col-sm-12 col-md-6 col-lg-4 mb-4" v-for="post in posts" :key="post.id">
        <div class="card">
          <div class="card-body">
            <h5 class="card-title">{{ post.title }}</h5>
            <h6 class="card-subtitle mb-2 text-muted">{{ formatData(post.date) }}</h6>
            <p class="card-text">{{ post.category.name }}</p>
            <p class="card-text" v-for="tag in post.tags" :key="tag.id">{{ tag.name }}</p>
          </div>
        </div>
      </div>
    </div>

    <div class="d-flex justify-content-between">
      <button class="btn btn-primary" @click="getApi(first_page_url)" :disabled="current_page == 1">Inizio</button>
      <div v-for="(link, index) in links" :key="index">
        <button class="btn btn-secondary mx-1" v-html="link.label" @click="getApi(link.url)"
          :disabled="link.active || !link.url"></button>
      </div>
      <button class="btn btn-primary" @click="getApi(last_page_url)" :disabled="current_page == last_page">Fine</button>
    </div>
  </div>
</template>


<style lang="scss">

@use './scss/style.scss';




</style>

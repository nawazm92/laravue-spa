<template>
    <card :title="$t('title_articles')">
      <div class="text-center">
        <div class="title mb-1 text-danger">
          Welcome to <strong>{{ title }}</strong> is a <strong>Single Page Applicatoin</strong> built with Laravel &amp; Vue.js
        </div>
        <hr/>
      </div>

      <div class="card card-body mb-2" v-for="article in articles" v-bind:key="article.id">
        <h4>
          <router-link :to="{ name: 'articles.view', params: { id: article.id } }">
            {{ article.title }}
          </router-link>
        </h4>
        <p>{{ article.body }}</p>
      </div>

      <div class="float-right">
        <nav aria-label="Page navigation example">
          <ul class="pagination">
            <li v-bind:class="[{disabled: !pagination.prev_page_url}]" class="page-item">
              <a class="page-link" href="javascript:;" @click="fetchArticles(pagination.prev_page_url)">Previous</a>
            </li>

            <li class="page-item disabled"><a class="page-link text-dark" href="#">Page {{ pagination.current_page }} of {{ pagination.last_page }}</a></li>
        
            <li v-bind:class="[{disabled: !pagination.next_page_url}]" class="page-item"><a class="page-link" href="javascript:;" @click="fetchArticles(pagination.next_page_url)">Next</a></li>
          </ul>
        </nav>
      </div>
    </card>
</template>

<script>
import { mapGetters } from 'vuex'

export default {
  metaInfo () {
    return { title: this.$t('home') }
  },

  data: () => ({
    title: window.config.appName,
    articles: [],
    article: {
      id: '',
      title: '',
      body: ''
    },
    article_id: '',
    pagination: {},
    edit: false
  }),

  computed: mapGetters({
    authenticated: 'auth/check'
  }),

  created() {
    this.fetchArticles();
  },

  methods: {
    fetchArticles(page_url) {
      let vm = this;

      page_url = page_url || '/api/articles';

      fetch(page_url)
        .then(res => res.json())
        .then(res => {
          this.articles = res.data;
          vm.makePagination(res.meta, res.links);
        })
        .catch(err => console.log(err));
    },
    makePagination(meta, links) {
      let pagination = {
        current_page: meta.current_page,
        last_page: meta.last_page,
        next_page_url: links.next,
        prev_page_url: links.prev
      };

      this.pagination = pagination;
    },
  }
}
</script>

<style scoped>
.top-right {
  position: absolute;
  right: 10px;
  top: 18px;
}

.title {
  font-size: 150%;
}

.card h4 a {
  color: #801336;
}
</style>

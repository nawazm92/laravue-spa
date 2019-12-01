<template>
  <card :title="article.title">
    <p>{{ article.body }}</p>
  </card>
</template>

<script>
export default {
  data() {
    return {
      article: {
        id: '',
        title: '',
        body: ''
      },
      article_id: '',
    };
  },
  created() {
    this.article_id = this.$route.params.id
    this.fetchArticle();
  },
  methods: {
    fetchArticle() {
      fetch('/api/article/' + this.article_id)
        .then(res => res.json())
        .then(res => {
          this.article = res.data;
        })
        .catch(err => console.log(err));
    }
  }
};
</script>
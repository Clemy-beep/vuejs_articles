<template>
  <transition appear>
    <div class="articles">
      <h1>
        <span class="material-icons">article </span> Liste de mes articles
      </h1>
      <p>{{ isLoading ? "Loading..." : "" }}</p>
      <transition-group tag="ul" name="articles-list">
        <div v-for="article in currentArticles" :key="article.id">
          <ArticleItem :article="article" />
          <router-link :to="{ name: 'article', params: { id: article.id } }">
            Voir l'article
          </router-link>
        </div>
      </transition-group>
    </div>
  </transition>
</template>

<script>
import ArticleItem from "./ArticleItem.vue";
import { mapWritableState, mapState } from "pinia";
import { useArticleStore } from "../../stores/article";

export default {
  data() {
    return {
      isLoading: true,
    };
  },
  components: { ArticleItem },
  mounted() {
    this.getArticles.length > 0 || this.fetchArticles();
  },
  computed: {
    ...mapWritableState(useArticleStore, {
      setArticles: "articles",
    }),

    ...mapState(useArticleStore, {
      getArticles: "articles",
    }),

    currentArticles() {
      return this.getArticles;
    },
  },
  methods: {
    async fetchArticles() {
      let articles = await fetch(
        "https://api.blog.quidam.re/api/getArticles.php"
      )
        .then((response) => response.json())
        .catch((e) => e);
      this.isLoading = false;
      if (articles instanceof Array) {
        this.setArticles = articles;
      }
    },
  },
};
</script>

<style scoped>
h1 {
  padding-bottom: 0.5em;
  font-weight: bold;
}

.articles-list-enter-active,
.articles-list-leave-active {
  transition: all 0.5s ease;
}
.articles-list-enter-from,
.articles-list-leave-to {
  opacity: 0;
  transform: translateX(30px);
}
.v-enter-active,
.v-leave-active {
  transition: opacity 0.5s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}
</style>

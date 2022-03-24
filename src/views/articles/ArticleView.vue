<template>
  <div>
    <ArticleItem :article="_article" />
  </div>
</template>

<script setup>
import ArticleItem from "../../components/articles/ArticleItem.vue";
import { useArticleStore } from "../../stores/article";
import { ref, onMounted, computed } from "vue";
import { useRoute } from "vue-router";

const _article = ref();
const articleStore = useArticleStore();
const route = useRoute();

const storedArticles = computed(() => {
  return articleStore.$state.articles;
});

onMounted(() => {
  let filtered_articles = undefined;
  if (storedArticles.value.length > 0) {
    filtered_articles = storedArticles.value.find(
      (article) => article.id === route.params.id
    );
  }
  if (filtered_articles) {
    _article.value = filtered_articles;
    return true;
  } else fetchArticle();

  async function fetchArticle() {
    console.log("hey");
    await fetch(
      `https://api.blog.quidam.re/api/getArticle.php?id=${route.params.id}`
    ).then((r) => {
      r.json().then((value) => {
        if (value instanceof Array) {
          _article.value = value[0];
        }
      });
    });
  }
});
</script>

<style scoped></style>

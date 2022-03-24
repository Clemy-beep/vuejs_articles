<template>
  <div>
    <AddArticle
      v-on:handle-submit="handleSubmit()"
      :article="a"
      :notification="notification"
      :new_article="new_article"
    />
  </div>
</template>

<script setup>
import AddArticle from "../../components/forms/AddArticle.vue";
import { ref } from "vue";

const a = ref({
  title: "",
  content: "",
  user_id: "",
  categorie: "",
});

var notification = ref("");
var new_article = ref(0);
async function handleSubmit() {
  let datas = new FormData();
  datas.append("title", a.value.title);
  datas.append("content", a.value.content);
  datas.append("user_id", a.value.user_id);
  datas.append("categorie", a.value.categorie);

  let response = await fetch("https://api.blog.quidam.re/api/postArticle.php", {
    method: "POST",
    body: datas,
  })
    .then((res) => res.json())
    .catch((err) => err);

  if (response.message === "") {
    notification = "Article ajouté avec succès";
    new_article = response.article_id;
  }
}
</script>

<style scoped></style>

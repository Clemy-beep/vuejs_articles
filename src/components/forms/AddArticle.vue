<template>
  <div>
    <transition name="fade" appear>
      <form
        class="d-flex justify-content-center align-items-center gap-2 flex-column text-light"
        @submit.prevent="$emit('handle-submit')"
      >
        <div>
          <label for="title"></label>
          <input type="text" v-model="art.title" id="title" />
        </div>
        <div>
          <label for="content"></label>
          <input type="text" v-model="art.content" id="content" />
        </div>
        <div>
          <label for="user_id"></label>
          <input type="number" v-model="art.user_id" id="user_id" />
        </div>
        <div>
          <label for="categorie"></label>
          <input type="number" v-model="art.categorie" id="categorie" />
        </div>
        <div>
          <input type="submit" value="Ajouter" />
        </div>
      </form>
    </transition>

    <div>
      <span> {{ notification }} </span>
      <span v-if="new_article">
        <router-link :to="{ name: 'article', params: { id: new_article } }">
          Voir l'article
        </router-link>
      </span>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    article: {},
    notification: String,
    new_article: Number,
  },
  data() {
    return {
      art: this.article,
    };
  },
};
</script>

<style scoped>
.d-flex {
  display: flex;
}
.flex-column {
  flex-direction: column;
}
.gap-2 {
  gap: 2;
}

.align-items-center {
  align-items: center;
}
.justify-content-center {
  justify-content: center;
}
.text-light {
  color: rgb(195, 192, 205) !important;
}

input {
  height: 40px;
  background-color: transparent;
  border: 1px solid #41b883;
  padding: 0.5em;
  margin: 0.5em;
  border-radius: 8px;
  color: rgb(195, 192, 205);
}

input[type="submit"] {
  background-color: #41b883;
  color: black;
  cursor: pointer;
}

input:active {
  outline-color: green;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 1s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>

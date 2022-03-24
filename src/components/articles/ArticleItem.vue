<template>
  <div>
    <h2>{{ article?.title }}</h2>
    <p>{{ article?.content }}</p>
  </div>
</template>
<script>
export default {
  props: {
    article: Object,
  },
  methods: {},
};
</script>

<style scoped></style>

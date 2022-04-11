<template>
  <div>
    <h1>{{ index.title }}</h1>
    <p>{{ index.description }}</p>
    <nuxt-content :document="index"/>
    <ul>
      <li v-for="(post, index) in posts" :key="index">
        <nuxt-link :to="post.path">{{ post.title }}</nuxt-link>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  async asyncData({ $content, params, error }) {
    const index = await $content("index")
      .fetch()
      .catch(err => {
        error({ statusCode: 404, message: "index not found" });
      });

    const posts = await $content("posts")
      .only(["title", "path"])
      .sortBy('title')
      .where({
        published: true
      })
      .fetch()
      .catch(err => {
        error({ statusCode: 404, message: "Página no encontrada" });
      });

    return {
      index,
      posts
    };
  }
};
</script>




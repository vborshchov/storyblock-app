<template>
  <section id="about-page" v-editable="content">
    <h1>{{ title }}</h1>
    <p>{{ content }}</p>
  </section>
</template>

<script>
  export default {
    transition: 'bounce',
    asyncData(context) {
      return context.app.$storyapi.get('cdn/stories/about', {
          version: context.isDev ? 'draft' : 'published',
        }).then(res => {
          console.log(res);
          return {
            blok: res.data.story.content,
            title: res.data.story.content.title,
            content: res.data.story.content.content
          }
        })
    },
    mounted() {
      this.$storyblok.init()
      this.$storyblok.on('change', () => {
          location.reload(true)
      })
    }
  }
</script>

<style scoped>
#about-page {
  width: 80%;
  max-width: 500px;
  margin: auto;
  padding-top: 2rem;
}

#about-page p {
  white-space: pre-line;
}
</style>
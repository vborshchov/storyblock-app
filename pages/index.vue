<template>
  <section id="posts">
    <PostPreview
      v-for="post in posts"
      :key="post.id"
      :title="post.title"
      :id="post.id"
      :thumbnailImage="post.thumbnailUrl"
      :excert="post.previewText" />
  </section>
</template>

<script>
import PostPreview from '@/components/Blog/PostPreview';

export default {
  asyncData(context) {
    return context.app.$storyapi.get('cdn/stories', {
      version: context.isDev ? 'draft' : 'published',
      starts_with: 'blog/'
    }).then(res => {
      console.log(res);
      return {
        posts: res.data.stories.map(bp => {
          return {
            id: bp.slug,
            title: bp.content.title,
            previewText: bp.content.summary,
            thumbnailUrl: bp.content.thumbnail
          }
        })
      }
    })
  },
  components: {
    PostPreview
  }
}
</script>

<style scoped>
  #posts {
    flex-wrap: wrap;
    padding-top: 2rem;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }

  @media (min-width: 35rem) {
    #posts {
      flex-direction: row;
    }
  }
</style>

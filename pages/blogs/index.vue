<template>
  <Main>
    <h1>Blogs</h1>
    <RowTags :tags="[...tags]" resource="blogs" />
    <RowBlogListItems :blogs="blogs" />
  </Main>
</template>

<script>
export default {
    data() {
    return {
      blogs: [],
      tags: []
    }
  },
  async fetch() {
    let doc = await this.$content('blogs')
    .only([
      'image', 'title', 'subtitle', 'url', 'by', 'date', 'tags', 'slug', 'author', 'excerpt'
    ])
    .sortBy('createdAt','desc')
    .fetch()

    const tags = new Set()

    if (doc.length > 0) {
      doc.forEach(item => {
        if (item.tags) {
          const listOfTags = typeof item.tags === 'object' ? item.tags : item.tags.split(',')
          listOfTags.forEach(tag => {
            tags.add(tag.trim())
          })
        }
      });
    }


    if (this.$route.query.tag) {
      doc = doc.filter(item => {
        return item.tags.includes(this.$route.query.tag)
      })
    }

    this.blogs = doc
    this.tags = [...tags]
  },
  watch: {
    '$route.query': '$fetch'
  }
}
</script>

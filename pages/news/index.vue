<template>
  <Main>
    <h1>News</h1>
    <RowTags :tags="[...tags]" resource="news" />
    <RowNewsCards :news="news" />
  </Main>
</template>

<script>
export default {
    data() {
    return {
      news: [],
      tags: []
    }
  },
  async fetch() {
    let doc = await this.$content('news')
    .only([
      'image', 'title', 'subtitle', 'url', 'by', 'date', 'tags', 'slug', 'excerpt'
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

    this.news = doc
    this.tags = [...tags]
  },
  watch: {
    '$route.query': '$fetch'
  }
}
</script>

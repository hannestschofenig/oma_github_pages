<template>
  <Main>
    <template v-for="(module, index) in document.modules">
      <RowText
        v-if="module.name === 'RowText'"
        :key="`${module.name}_${index}`"
        :text="module.text"
        :status="module.status"
      />
      <RowTextImage
        v-else-if="module.name === 'RowTextImage'"
        :key="`${module.name}_${index}`"
        :cols="module.cols"
        :text="module.text"
        :status="module.status"
        :image="module.image" />
      <RowImageText
        v-else-if="module.name === 'RowImageText'"
        :key="`${module.name}_${index}`"
        :cols="module.cols"
        :text="module.text"
        :status="module.status"
        :image="module.image"
      />
      <RowImage
        v-else-if="module.name === 'RowImage'"
        :key="`${module.name}_${index}`"
        :title="module.title"
        :image="module.image"
        :class="module.status"
      />
      <RowDynamicTable
        v-else-if="module.name === 'RowDynamicTable'"
        :key="`${module.name}_${index}`"
        :config="module"
      />
      <RowButtonGrid
        v-else-if="module.name === 'RowButtonGrid'"
        :key="`${module.name}_${index}`"
        :config="module"
      />
      <RowSeparator
        v-else-if="module.name === 'RowSeparator'"
        :key="`${module.name}_${index}`"
        :size="module.size"
      />
    </template>
    <Default :document="document" />
  </Main>
</template>

<script>
export default {
  data() {
    return {
      document: {
        modules: []
      }
    }
  },
  async fetch() {
    const doc = await this.$content(this.$route.params.slug).fetch()
    this.document = doc
  }
}
</script>

<template>
  <div>
    <h1 class="title">Add Tag to User</h1>

    <h2 class="subtitle">
      and press Enter
    </h2>

    <div class="summary">
      <vue-tags-input
      v-model="tag"
      :tags="tags"
      @tags-changed="newTags => tags = newTags"
    />
    </div>
  </div>
</template>

<script>
import VueTagsInput from '@johmun/vue-tags-input';
  export default {
    components: {
       VueTagsInput,
    },
    data () {
      return {
        tag: '',
        tags: []
      }
    },
    methods: {
      submit () {
        return Promise.resolve({
          tag: this.tags
        })
      }
    }
  }
</script>

<style scoped>

</style>
<template>
  <section class="container">
    <ImageSearch :images.sync="images" :search_status.sync="search_status" :query.sync="query"></ImageSearch>
    <ImageList :images.sync="images"></ImageList>
  </section>
</template>

<script>
  import ImageSearch from '~/compontents/images/ImageSearch.vue'
  import ImageSearch from '~/compontents/images/ImageList.vue'

  export default {
    data: function() {
      return {
        query: "",
        images: [],
        active_modal: 0,
        page: 1,
        search_status: false
      }
    }
  }
</script>

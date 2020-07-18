<template>
  <div class="columns is-multiline">
    <div v-for="image in images" :key="image.id" class="column is-4">
      <image-card></image-card>
      <image-modal></image-modal>
      <infinite-loading v-if="searchStatus()" ref="infiniteLoading" spinner="waveDots" @infinite="infiniteHandler"></infinite-loading>
    </div>
  </div>
</template>

<script>
export default {
  props: ['images'],
  methods: {
    infiniteHandler: function($state) {
      this.page += 1;
      this.$axios.$get(`/api/images?q=${this.query}&page=${this.page}`)
        .then(json => {
          if(json.length > 0) {
            this.images.push(...json);
            $state.loaded();
          } else {
            $state.complete();
            this.search_status = false;
          }
        })
        .catch(e => ({ error: e }))
    },
    searchStatus: function() {
      return this.search_status;
    }
  }
}
</script>

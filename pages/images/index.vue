<template>
  <section class="container">
    <div class="columns">
      <input type="text" placeholder="キーワード" v-model="query">
      <button v-on:click.prevent="searchImages()" class="button">検索</button>
    </div>
    <div class="columns is-multiline">
      <div v-for="image in images" :key="image.id" class="column is-3">
        <img :src="image.image_url">
      </div>
    </div>
  </section>
</template>

<script>
  export default {
    data: function() {
      return {
        query: "",
        images: []
      }
    },
    methods: {
      searchImages: function() {
        this.$axios.$get(`/api/images?q=${this.query}`)
          .then(json => {
            console.log(json[0])
            this.images = json
          })
          .catch(e => ({ error: e }))
      }
    }
  }
</script>

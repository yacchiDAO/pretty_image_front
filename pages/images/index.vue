<template>
  <section class="container">
    <div class="columns">
      <input type="text" placeholder="キーワード" v-model="query">
      <button v-on:click.prevent="searchImages()" class="button">検索</button>
    </div>
    <div class="columns is-multiline">
      <div v-for="image in images" :key="image.id" class="column is-3">

        <div class="card">
          <div class="card-image">
            <figure class="image is-16by9">
              <img :src="image.image_url">
            </figure>
          </div>
          <div class="card-content">
            <div class="media">
              <div class="media-left">
                <figure class="image is-128x128">
                  <img :src="image.animation.image_url">
                </figure>
              </div>
              <div class="media-content">
                <p class="title is-4">{{image.line}}</p>
                <p class="subtitle is-6">{{image.characters.map(function (character) {return character.name}).join('/')}}</p>
              </div>
            </div>

            <div class="content">
              {{image.description}}
            </div>
          </div>
        </div>

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
            this.images = json
          })
          .catch(e => ({ error: e }))
      }
    }
  }
</script>

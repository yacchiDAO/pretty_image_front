<template>
  <section class="container">
    <div class="columns">
      <input type="text" placeholder="キーワード" v-model="query">
      <button v-on:click="searchImages()" class="button">検索</button>
    </div>
    <div class="columns is-multiline">
      <div v-for="image in images" :key="image.id" class="column is-3">
        <div v-on:click="active_modal=image.id" class="card">
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
                <p class="title is-6">{{image.line}}</p>
                <p class="subtitle is-6">{{image.characters.map(function (character) {return character.name}).join('/')}}</p>
              </div>
            </div>

            <div class="content">
              {{image.description}}
            </div>
          </div>
        </div>
        <div :id="'modal' + image.id" class="modal" v-bind:class="[active_modal === image.id ? 'is-active' : '' ]">
          <div class="modal-background" v-on:click="active_modal=0"></div>
          <div class="modal-content">
            <p class="image is-16by9">
              <img :src="image.image_url">
            </p>
          </div>
          <button class="modal-close is-large" aria-label="close" v-on:click="active_modal=0"></button>
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
        images: [],
        active_modal: 0
      }
    },
    methods: {
      searchImages: function() {
        this.$axios.$get(`/api/images?q=${this.query}`)
          .then(json => {
            this.images = json
          })
          .catch(e => ({ error: e }))
      },
      activeModal: function(image_id) {
        return false;
      }
    }
  }
</script>

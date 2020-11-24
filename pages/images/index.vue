<template>
  <section class="container">
    <div class="columns">
      <input type="text" placeholder="キーワード" v-model="query">
      <button v-on:click="searchImages()" class="button">検索</button>
    </div>
    <div class="columns is-multiline">
      <div v-for="image in images" :key="image.id" class="column is-4">
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
                <p class="title is-6">{{imageLine(image.line)}}</p>
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

    <infinite-loading v-if="searchStatus()" ref="infiniteLoading" spinner="waveDots" @infinite="infiniteHandler">
      <span slot="no-more"/>
      <span slot="no-results"/>
    </infinite-loading>
  </section>
</template>

<script>
  export default {
    data: function() {
      return {
        query: "",
        images: [],
        active_modal: 0,
        page: 1,
        search_status: false
      }
    },
    methods: {
      searchImages: function() {
        if(this.query != "") {
          this.$nuxt.$loading.start();
          this.page = 1;
          this.$axios.$get(`/api/images?q=${this.query}&page=${this.page}`)
            .then(json => {
              this.images = json;
              this.search_status = true;
            })
            .catch(e => ({ error: e }))
          this.$nuxt.$loading.finish();
          if(this.$refs.InfiniteLoading){
            this.$refs.infiniteLoading.stateChanger.reset();
          }
        }
      },
      imageLine: function(line) {
        return(line == "" ? 'セリフなし' : line);
      },
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

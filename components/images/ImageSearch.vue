<template>
  <div class="columns">
    <input type="text" placeholder="キーワード" v-model="query">
    <button v-on:click="searchImages()" class="button">検索</button>
  </div>
</template>

<script>
  export default {
    props: {
      images: List,
      search_status: Boolean,
      query: String
    },
    computed: {
      images: {
        get() {
          return this.images
        },
        set(value) {
          this.$emit('update:images', value)
        }
      },
      search_status: {
        get() {
          return this.search_status
        },
        set(value) {
          this.$emit('update:search_status', value)
        }
      },
      query: {
        get () {
          return this.query
        },
        set(value) {
          this.$emit('update:query', value)
        }
      }
    },
    methods: {
      // TODO: storeにぶちこむ
      searchImages: function() {
        if(this.query != "") {
          this.$nuxt.$loading.start();
          this.page = 1;
          this.$axios.$get(`/api/images?q=${this.query}`)
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
      }
    }
  }
</script>

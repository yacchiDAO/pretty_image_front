<template>
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
</template>

<script>
export default {
  props: ['image'],
  mothods: {
    imageLine: function(line) {
      return(line == "" ? 'セリフなし' : line);
    }
  }
}
</script>

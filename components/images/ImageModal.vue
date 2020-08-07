<template>
  <div :id="'modal' + image.id" class="modal" v-bind:class="[active_modal === image.id ? 'is-active' : '' ]">
    <div class="modal-background" v-on:click="active_modal=0"></div>
    <div class="modal-content">
      <p class="image is-16by9">
        <img :src="image.image_url">
      </p>
    </div>
    <button class="modal-close is-large" aria-label="close" v-on:click="active_modal=0"></button>
  </div>
</template>

<script>
export default {
  props: ['image']
}
</script>

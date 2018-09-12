<template>
  <div
    class="overlay"
    v-bind:class="{ visible: visible }"
  >
    <h3
      class="title"
      v-bind:style="{color: color}"
    >
      {{ title }}
    </h3>

    <p class="message">
      {{ message }}
    </p>
  </div>
</template>

<script>
export default {
  props: {
    visible: {
      type: Boolean,
      default: false
    },
    title: {
      type: String,
      required: true,
    },
    message: {
      type: String
    },
    color: {
      type: String,
      default: 'inherit'
    }
  }
}
</script>

<style scoped>
  .overlay {
    background: #fff;
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    opacity: 0;
    visibility: hidden;
    z-index: 100;
    display: flex;
    flex-direction: column;
    justify-content: center;
    padding: 1em;
    text-align: center;
  }

  .overlay.visible {
    opacity: 1;
    transition: opacity 0.4s;
    visibility: visible;
  }

  .title {
    font-family: 'Roboto Slab', serif;
    font-weight: normal;
    font-size: 1.5em;
    margin-bottom: 0.5em;
  }

  .message {
    margin: 0;
  }
</style>

<template>
  <div class="menu">
    <MenuItem v-for="item in items" v-bind:key="item.id" v-bind:item="item" />
  </div>
</template>

<script>
  import MenuItem from './MenuItem.vue'
  import axios from 'axios'

  export default {
    data: function() {
      return {
        items: []
      };
    },
    props: {
      feed: {
        type: String,
        required: true
      }
    },
    components: {
      MenuItem
    },
    created: function() {
      axios.get(this.feed)
        .then((response) => {
          this.items = response.data.menu.items
        })
    }
  }
</script>

<style scoped>
  .item-title {
    color: #555;
    font-family: 'Abril Fatface', cursive;
    font-size: 1.5em;
    font-weight: normal;
    line-height: 1.1;
    margin: 0;
  }

  .menu {
    display: grid;
    grid-gap: 20px;
    grid-template-columns: 1fr;
    padding: 15px;
  }

  .menu-item {
    background: #fff;
    box-shadow: 1px 1px 6px #eee;
    display: flex;
    flex-direction: column;
    position: relative;
  }

  .menu-item .text-wrapper {
    padding: 1em;
  }

  .price {
    color: #5dc0a3;
    font-size: 1.25em;
    font-weight: bold;
    margin-bottom: 0;
  }

  /* 1 narrow column */
  @media screen and (min-width: 405px) {
    .image-wrapper img {
      height: 100%;
      object-fit: cover;
      object-position: 80%;
      width: 100%;
    }

    .image-wrapper {
      background: linear-gradient(
        115deg, rgba(255,255,255,1) 0%,
        rgba(255,255,255,1) 50%,
        rgba(161,218,201,1) 50%,
        rgba(161,218,201,1) 100%
      );
      flex: 1 1 40%;
    }

    .text-wrapper {
      flex: 1 1 60%;
    }

    .menu-item {
      background-image: inherit;
      flex-direction: row-reverse;
    }
  }

  /* 1 wide column */
  @media screen and (min-width: 530px) {
    .image-wrapper img {
      object-position: 100%;
    }

    .item-title {
      font-size: 2em;
    }

    .image-wrapper {
      flex: 1 1 50%;
    }

    .text-wrapper {
      flex: 1 1 50%;
    }
  }

  /* 2 narrow columns */
  @media screen and (min-width: 720px) {
    .menu {
      grid-template-columns: 1fr 1fr;
    }

    .image-wrapper img {
      object-position: 80%;
    }

    .item-title {
      font-size: 1.5em;
    }

    .image-wrapper {
      flex: 1 1 40%;
    }

    .text-wrapper {
      flex: 1 1 60%;
    }
  }

  /* 2 wide column */
  @media screen and (min-width: 900px) {
    .image-wrapper img {
      object-position: 100%;
      object-position: 80%;
    }

    .item-title {
      font-size: 2em;
    }

    .image-wrapper {
      flex: 1 1 50%;
    }

    .text-wrapper {
      flex: 1 1 50%;
    }
  }

  /* 3 narrow columns */
  @media screen and (min-width: 1205px) {
    .menu {
      grid-template-columns: 1fr 1fr 1fr;
    }

    .image-wrapper img {
      object-position: 80%;
    }

    .item-title {
      font-size: 1.5em;
    }

    .image-wrapper {
      flex: 1 1 40%;
    }

    .text-wrapper {
      flex: 1 1 60%;
    }
  }

  @media screen and (min-width: 1400px) {
    .image-wrapper img {
      object-position: 100%;
    }

    .item-title {
      font-size: 2em;
    }

    .image-wrapper {
      flex: 1 1 50%;
    }

    .text-wrapper {
      flex: 1 1 50%;
    }
  }
</style>

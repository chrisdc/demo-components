<template>
  <Menu
    title="My Menu"
    feed="https://dev.menu.ninja/api/menu/156?key=8j5vfe%24*pfb**rzt&pretty=1"
  />
</template>

<script>
import Menu from './components/Menu.vue'

export default {
	components: {
		Menu
	}
}
</script>

<style>
  html {
    background: #f9f9f9;
    box-sizing: border-box;
    font-family: 'Lato', sans-serif;
    padding: 0;
  }

  body {
    background: #f9f9f9;
  }

  * {
    box-sizing: border-box;
  }

  img {
    width: 100%;
    height: auto;
    vertical-align: top;
  }
</style>

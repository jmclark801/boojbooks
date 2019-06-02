<template>
  <div id="app">
    <img alt="booj logo" src="./assets/booj-triangle.png">
    <Books/>
  </div>
</template>

<script>
import Books from './components/Books.vue'

export default {
  name: 'app',
  components: {
    Books
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Montserrat:400,700');

#app {
    width: 50%;
}

body {
  /* background-color: #777; */
  background-color: #7DD;
  font-family: 'Montserrat', sans-serif;
  display: grid;
  grid-template-rows: auto;
  justify-items: center;
  align-items: center;
}

body, html {
  margin: 0;
  height: 100%;
}

img {
  display: block;
  margin-left: auto;
  margin-right: auto;
  margin-bottom: 25px;
  margin-top: 25px;
  width: 35%;
}
</style>

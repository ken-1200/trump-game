<template>
  <div id="app">
    <Home></Home>
    <div class="container">
      <div class="box-items" v-for="n in 52" :key="n">
        <img :src="images(n)">
    </div>
    </div>
  </div>
</template>

<script>
import Home from './components/Home.vue'

export default {
  methods: {
    images(n) {
      return require(`../src/assets/images/trump/${n}.gif`)
    }
  },
  components: {
    Home
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  list-style-type: none;
  margin-top: 60px;
}
.box-items {
  margin: 0 10px;
  padding: 10px 0;
}

img {
  width: 100%;
}
</style>

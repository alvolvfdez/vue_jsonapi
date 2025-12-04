<template>
  <div id="app">
    <b-navbar type="dark" variant="info">
      <b-navbar-nav>
        <b-nav-item :to="{ name: 'Articles' }">Artículos</b-nav-item>
      </b-navbar-nav>
    </b-navbar>
    <router-view/>
  </div>
</template>

<script>
export const restConfig = {
  headers: {
    'Content-Type': 'application/ json',
    'X-CSRF-Token': 'Nzemkyhz2hJaKOHsg1JpZQTJDRWFTXFleQJWWbXA32s',
    'Authorization': 'Basic ' + btoa('usuariodemo:usuariodemo')
  },
  params: {
    '_format': 'json'
  }
}
export const siteDomain = 'http://fe3.alvaolve.training.forcontu.com/'

export default {
  name: 'App'
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
.navbar {
  margin-bottom: 20px;
}
</style>

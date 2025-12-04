<template>
  <div class="articles">
    <b-card-group columns id="list">
      <article-card v-for="article in articles"
                    v-bind:key="article.uuid"
                    v-bind:article="article"
      ></article-card>
    </b-card-group>
  </div>
</template>
<script>
import ArticleCard from '@/components/ArticleCard'
import {jsonApiConfig, siteDomain} from '@/App'
import axios from 'axios'

export default {
  name: 'Articles',
  data () {
    return {
      articles: []
    }
  },
  created () {
    axios
      .get(siteDomain + '/jsonapi/node/article', jsonApiConfig)
      .then(response => (this.articles = response.data))
  },
  components: {
    ArticleCard
  }
}

</script>
<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  padding: 10px;
}
</style>

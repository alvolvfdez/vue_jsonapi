<template>
  <div class="articles">
    <b-card-group columns id="list">
      <article-card v-for="article in articles"
                    v-bind:key="article.id"
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
      articles: [],
      included: []
    }
  },
  created () {
    axios
      .get(siteDomain + 'jsonapi/node/article?include=img', jsonApiConfig)
      .then(response => {
        this.articles = response.data.data
        this.included = response.data.included
      })
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

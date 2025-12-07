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
        const data = response.data.data
        const included = response.data.included || []

        // Recorremos cada artículo para asignarle su imagen
        this.articles = data.map(article => {
          // Si el artículo tiene relación con imagen
          if (article.relationships && article.relationships.img && article.relationships.img.data) {
            const imgId = article.relationships.img.data.id
            // Buscamos la imagen en el array included
            const img = included.find(i => i.id === imgId)
            if (img) {
              // Inyectamos la URL directamente en el objeto article para usarlo fácil en el hijo
              article.imgUrl = img.attributes.uri.url
            }
          }
          return article
        })
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

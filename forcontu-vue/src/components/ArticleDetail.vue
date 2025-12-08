<template>
  <div class="article-detail">
    <p v-if="loading">Loading...</p>
    <b-container v-else>
      <b-row>
        <b-col cols="12"><h1>{{ article.attributes.title }}</h1></b-col>
      </b-row>
      <b-row>
        <b-col cols="12">
          <!-- Usamos article.imgUrl directamente, que preparamos en el created -->
          <div class="image float-left" v-if="article.imgUrl">
            <img v-bind:src="siteDomain + article.imgUrl" v-bind:alt="article.attributes.title" />
          </div>
          <div v-html="article.attributes.body.value"></div>
        </b-col>
      </b-row>
      <b-row>
        <b-col cols="12">
          <p>
            Enviado el {{ formatDate(article.attributes.created) }}
          </p>

          <p v-if="article.attributes.changed !== article.attributes.created">
            Última modificación: {{ formatDate(article.attributes.changed) }}
          </p>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>
<script>
import {jsonApiConfig, siteDomain} from '@/App'
import axios from 'axios'

export default {
  name: 'article-detail',
  data () {
    return {
      article: null, // Inicializamos como null
      siteDomain: siteDomain,
      loading: false
    }
  },
  created () {
    this.loading = true
    axios
      .get(siteDomain + 'jsonapi/node/article/' + this.$route.params.id + '?include=img', jsonApiConfig)
      .then(response => {
        const data = response.data.data
        const included = response.data.included || []

        // Procesamos la imagen igual que hicimos en Articles.vue
        if (data.relationships && data.relationships.img && data.relationships.img.data) {
          const imgId = data.relationships.img.data.id
          const img = included.find(i => i.id === imgId)
          if (img) {
            data.imgUrl = img.attributes.uri.url
          }
        }

        this.article = data
        this.loading = false
      })
      .catch(error => {
        console.error(error)
        this.loading = false
      })
  },
  methods: {
    formatDate (unix) {
      const date = new Date(unix * 1000)
      const day = ('0' + date.getDate()).slice(-2)
      const month = ('0' + (date.getMonth() + 1)).slice(-2)
      const year = date.getFullYear()
      const hours = ('0' + date.getHours()).slice(-2)
      const minutes = ('0' + date.getMinutes()).slice(-2)

      return day + '/' + month + '/' + year + ' - ' + hours + ':' + minutes
    }
  }
}
</script>
<style scoped>
image {
  padding: 20px;
}
</style>

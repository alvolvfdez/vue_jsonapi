<template>
  <div class="article-detail">
    <p v-if="loading">Loading...</p>
    <b-container v-else>
      <b-row>
        <b-col cols="12"><h1>{{ $route.params.uuid }} - {{article.title}}</h1></b-col>
      </b-row>
      <b-row>
        <b-col cols="12">
          <div class="image float-left"><img v-bind:src="siteDomain + article.img" v-bind:alt="article.title" /></div>
          <div v-html="article.text"></div>
        </b-col>
      </b-row>
      <b-row>
        <b-col cols="12">
          <p>
            Enviado por {{ article.user }}
            el {{ formatDate(article.created) }}
          </p>

          <p v-if="article.changed !== article.created">
            Última modificación: {{ formatDate(article.changed) }}
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
      article: [],
      siteDomain: siteDomain,
      loading: false
    }
  },
  created () {
    this.loading = true
    axios
      .get(siteDomain + '/jsonapi/node/article/' + this.$route.params.id, jsonApiConfig)
      .then(response => {
        this.article = response.data.data
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

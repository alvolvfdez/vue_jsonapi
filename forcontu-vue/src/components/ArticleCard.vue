<template>
  <div class="article-card">
    <b-card
      v-bind:title="article.title"
      v-bind:img-src="siteDomain + article.img"
      v-bind:img-alt="article.title"
      img-top
      tag="article"
      style="max-width: 20rem;"
      class="mb-2">
      <b-card-text v-html="article.text"></b-card-text>
      <b-card-text>
        {{ article.body }}
      </b-card-text>
      <b-button variant="primary" :to="{ name: 'ArticleDetail', params: { id: article.id }}">Ver más</b-button>
    </b-card>
  </div>
</template>
<script>
import {siteDomain} from '@/App'

export default {
  name: 'article-card',
  props: ['article'],
  data () {
    return {
      siteDomain: siteDomain
    }
  }

}
</script>
<style scoped>
</style>

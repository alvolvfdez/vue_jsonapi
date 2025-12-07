<template>
  <div class="article-card">
    <b-card
      v-bind:title="article.attributes.title"
      v-bind:img-src="siteDomain + article.attributes.img"
      v-bind:img-alt="article.attributes.title"
      img-top
      tag="article"
      style="max-width: 20rem;"
      class="mb-2">
      <b-card-text v-html="article.attributes.text.processed"></b-card-text>
      <b-card-text>
        {{ article.attributes.text.summary }}
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

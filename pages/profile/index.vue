<template>
  <div class="profile-page">

    <div class="user-info">
      <div class="container">
        <div class="row">

          <div class="col-xs-12 col-md-10 offset-md-1">
            <img :src="profile.image" class="user-img" />
            <h4>{{ profile.username }}</h4>
            <p>
              {{ profile.bio }}
            </p>
            <button class="btn btn-sm btn-outline-secondary action-btn">
              <i class="ion-plus-round"></i>
              &nbsp;
              Follow {{ profile.username }}
            </button>
          </div>

        </div>
      </div>
    </div>

    <div class="container">
      <div class="row">

        <div class="col-xs-12 col-md-10 offset-md-1">
          <div class="articles-toggle">
            <ul class="nav nav-pills outline-active">
              <li class="nav-item">
                <nuxt-link
                  class="nav-link"
                  :class="{
                    active: tab==='my'
                  }"
                  exact
                  :to="{
                    path: '/profile/'+ profile.username,
                    query: {
                      tab: 'my'
                    }
                  }"
                >My Articles</nuxt-link>
              </li>
              <li class="nav-item">
                <nuxt-link
                  class="nav-link"
                  :class="{
                    active: tab==='favorited'
                  }"
                  exact
                  :to="{
                    path: '/profile/'+ profile.username,
                    query: {
                      tab: 'favorited'
                    }
                  }"
                >Favorited Articles</nuxt-link>
              </li>
            </ul>
          </div>

          <div v-for="article in articles" :key="article.createdAt" class="article-preview">
            <div class="article-meta">
              <a href=""><img :src="article.author.image" /></a>
              <div class="info">
                <a href="" class="author">{{ article.author.username }}</a>
                <span class="date">{{ article.createdAt | date('MMM DD, YYYY') }}</span>
              </div>
              <button class="btn btn-outline-primary btn-sm pull-xs-right"
              :class="{
                  active: article.favorited
                }"
                disabled
              >
                <i class="ion-heart"></i> {{ article.favoritesCount }}
              </button>
            </div>
            <a href="" class="preview-link">
              <h1>{{ article.title }}</h1>
              <p>{{ article.description }}</p>
              <span>Read more...</span>
              <ul class="tag-list">
                <li class="tag-default tag-pill tag-outline" v-for="tag in article.tagList" :key="tag">{{ tag }}</li>
              </ul>
            </a>
          </div>
        </div>

      </div>
    </div>

  </div>
</template>

<script>
import { getProfile } from '@/api/profile'
import { getArticles } from '@/api/article'

export default {
  middleware: 'authenticated',
  name: 'UserProfile',
  async asyncData ({ params,query }) {
    const { profile } = (await getProfile(params.username)).data

    const tab = query.tab || 'my'
    const isForMyArticle = tab ==='my'

    const { articles } = (await getArticles({
      [isForMyArticle?'author':'favorited']: params.username
    })).data

    return {
      profile,
      isForMyArticle,
      articles,
      tab
    }
  },
  watchQuery:['tab'],
}
</script>

<style>

</style>

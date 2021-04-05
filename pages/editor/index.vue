<template>
  <div class="editor-page">
    <div class="container page">
      <div class="row">

        <div class="col-md-10 offset-md-1 col-xs-12">
          <form @submit.prevent="onSubmit">
              <fieldset class="form-group">
                  <input type="text" v-model="title" class="form-control form-control-lg" placeholder="Article Title" required>
              </fieldset>
              <fieldset class="form-group">
                  <input type="text" v-model="description" class="form-control" placeholder="What's this article about?" required>
              </fieldset>
              <fieldset class="form-group">
                  <textarea class="form-control" v-model="body" rows="8" placeholder="Write your article (in markdown)" required></textarea>
              </fieldset>
              <fieldset class="form-group">
                  <input type="text" class="form-control" placeholder="Enter tags"><div class="tag-list"></div>
              </fieldset>
              <button class="btn btn-lg pull-xs-right btn-primary" type="submit">
                  Publish Article
              </button>
          </form>
        </div>

      </div>
    </div>
  </div>
</template>

<script>
import { createArticle } from '@/api/article'

export default {
  // 在路由匹配组件渲染之前会先执行中间件处理
  middleware: 'authenticated',
  name: 'EditorIndex',
  data(){
    return {
      title:'',
      description:'',
      body:''
    }
  },
  methods:{
    async onSubmit(){
      await createArticle({
        title: this.title,
        description: this.description,
        body: this.body
      })
      this.$router.push('/')
    }
  }
}
</script>

<style>

</style>

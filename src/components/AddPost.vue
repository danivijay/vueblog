<template>
  <v-layout row wrap>
    <v-flex
      white
      elevation-2
      xs12
      sm10 
      offset-sm1
      >
      <v-toolbar flat dense class="grey darken-4" dark>
        <v-toolbar-title>Add a new post</v-toolbar-title>
      </v-toolbar>
      <v-form 
        v-if="!blog.posted"
        class="pr-3 pl-3"
        ref="form" 
        lazy-validation>
        <v-text-field
          label="Title"
          v-model.lazy="blog.title"
          :rules="blog.titleRules"
          required
        ></v-text-field>
        <v-text-field
          label="Content"
          v-model.lazy="blog.content"
          :rules="blog.contentRules"
          :counter="280"
          required
          multi-line
        ></v-text-field>
        <v-checkbox label="Web Development" v-model="blog.categories" value="Web Development"></v-checkbox>
        <v-checkbox label="Entreprenuership" v-model="blog.categories" value="Entreprenuership"></v-checkbox>
        <v-checkbox label="Productivity" v-model="blog.categories" value="Productivity"></v-checkbox>
        <v-select
          v-bind:items="blog.levelTypes"
          v-model="blog.level"
          label="Auther"
          single-line
          bottom
        ></v-select>
        <v-btn
          @click="submitPost"
        >
          Post now
        </v-btn>
      </v-form>

      <v-alert 
        v-if="blog.posted" 
        class="ml-2 mr-2 mt-2 mb-2"
        color="success" 
        icon="check_circle" 
        value="true">
      Successfully posted
      </v-alert>

      <v-alert v-if="failed" class="ml-2 mr-2 mt-2 mb-2" color="error" icon="warning" value="true">
      Oops! Please try again
      </v-alert>

      <v-divider></v-divider>

      <div class="pr-3 pl-3 pb-3 pt-3">
        <h5>Preview:</h5>
        <h2>{{ blog.title }}</h2>
        <p> {{ blog.level.text }}</p>
        <p>{{ blog.content }}</p>
        <v-chip v-for="category in blog.categories" :key="category"> {{ category }} </v-chip>
      </div>
    </v-flex>
  </v-layout>
</template>

<script>
import axios from 'axios'

export default {
  data () {
    return {
      blog: {
        title: '',
        titleRules: [
          (v) => !!v || 'Title is required',
          (v) => v && v.length >= 3 || 'Title must be greater than 3 characters',
          (v) => v && v.length <= 20 || 'Title must be less than 20 characters'
        ],
        content: '',
        contentRules: [
          (v) => !!v || 'Content is required',
          (v) => v && v.length >= 3 || 'Content must be greater than 3 characters',
          (v) => v && v.length <= 280 || 'Content must be less than 280 characters'
        ],
        categories: [],
        levelTypes: [
          { text: 'Dani' },
          { text: 'UtmostDev' },
          { text: 'The Web Club' }
        ],
        level: '',
        posted: false
      },
      failed: false
    }
  },
  methods: {
    async submitPost () {
      try {
        const data = (await axios.post(`https://vueblog-1822a.firebaseio.com/posts.json`, this.blog)).data
        this.blog.posted = true
        console.log(this.blog.posted)
      } catch (e) {
        this.failed = true
        console.log(e)
      }
    }
  }
}
</script>

<style>

</style>

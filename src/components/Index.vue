<template>
  <q-layout>
    <div slot="header" class="toolbar">
      <q-toolbar-title :padding="0">
        Information Retrieval
      </q-toolbar-title>
    </div>

    <div class="layout-view">
      <div class="layout-padding">
        <q-search v-model="searchQuery" :debounce="1000" @input="search"></q-search>
        <table class="q-table">
          <thead>
            <tr>
              <th class="text-left">Rating</th>
              <th class="text-left">Tail</th>
              <th class="text-left">Title</th>
              <th class="text-left">Review</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="result in results">
              <td>{{ result.rating }}</td>
              <td>{{ result.tail }}</td>
              <td>{{ result.title }}</td>
              <td>{{ result.review }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </q-layout>
</template>

<script>
  'use strict'

  import { Loading } from 'quasar'
  import axios from 'axios'

  export default {
    data () {
      return {
        searchQuery: '',
        results: []
      }
    },
    methods: {
      search () {
        Loading.show({
          delay: 100,
          message: 'Searching...',
          spinner: 'dots',
          spinnerSize: 150
        })

        const config = {
          method: 'get',
          baseURL: 'http://localhost:8000/api',
          url: `/search?search_query=${this.searchQuery}`
        }

        axios(config)
          .then(response => {
            this.results = response.data
            Loading.hide()
          })
          .catch(error => {
            throw new Error(error)
          })
      }
    }
  }
</script>

<style lang="styl">
.logo-container
  width 192px
  height 268px
  perspective 800px
  position absolute
  top 50%
  left 50%
  transform translateX(-50%) translateY(-50%)
.logo
  position absolute
  transform-style preserve-3d
</style>

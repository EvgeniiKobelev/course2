<template>
  <div class="container column">
    <app-form @addBlock="createBlock"></app-form>
    <app-rezume :blocks="blocks" :alert="alert"></app-rezume>
  </div>

  <app-comments :comments="comments" @load="loadComments"></app-comments>
  <div class="loader" v-if="loading"></div>
  <div v-if="alert !== 0" v-text="alert"></div>
</template>

<script>
import AppRezume from './components/AppRezume'
import axios from 'axios'
import AppComments from './components/AppComments.vue'
import AppForm from './components/AppForm.vue'
export default {
  components: { AppRezume, AppComments, AppForm },
  data () {
    return {
      blocks: [],
      comments: [],
      loading: false,
      alert: ''
    }
  },
  mounted () {
    this.loadBlocks()
  },
  methods: {
    async loadComments () {
      try {
        this.loading = true
        const { data } = await axios.get(
          'https://jsonplaceholder.typicode.com/comments?_limit=42'
        )
        this.comments = Object.keys(data).map(key => {
          return {
            id: key,
            ...data[key]
            // firstName: data[key].firstName
          }
        })
        this.loading = false
      } catch (e) {
        this.loading = false
        this.alert = 'Что-то пошло не так'
      }
    },
    async loadBlocks () {
      try {
        const { data } = await axios.get(
          'https://vuejs-with-http-default-rtdb.firebaseio.com/rezume.json'
        )
        this.blocks = Object.keys(data).map(key => {
          return {
            id: key,
            ...data[key]
            // firstName: data[key].firstName
          }
        })
      } catch (e) {
        this.loading = false
        this.alert = 'Что-то пошло не так'
      }
    },
    async createBlock (v) {
      const response = await fetch(
        'https://vuejs-with-http-default-rtdb.firebaseio.com/rezume.json',
        {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({
            typeBlock: v.typeBlock,
            valueBlock: v.valueBlock
          })
        }
      )
      const firebaseData = await response.json()
      this.blocks.push({
        typeBlock: v.typeBlock,
        valueBlock: v.valueBlock,
        id: firebaseData.name
      })
    }
  }
}
</script>

<style>
.avatar {
  display: flex;
  justify-content: center;
}

.avatar img {
  width: 150px;
  height: auto;
  border-radius: 50%;
}
</style>

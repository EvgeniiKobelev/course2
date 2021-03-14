<template>
  <div class="container column">
    <form class="card card-w30" @submit.prevent="createBlock">
      <div class="form-control">
        <label for="type">Тип блока</label>
        <select id="type" v-model="type">
          <option value="title">Заголовок</option>
          <option value="subtitle">Подзаголовок</option>
          <option value="avatar">Аватар</option>
          <option value="text">Текст</option>
        </select>
      </div>

      <div class="form-control">
        <label for="value">Значение</label>
        <textarea id="value" rows="3" v-model.trim="value"></textarea>
      </div>

      <button type="submit" class="btn primary" :disabled="value.length <= 3">
        Добавить
      </button>
    </form>
    <app-rezume :blocks="blocks" :type="type" :alert="alert"></app-rezume>
  </div>

  <app-comments :comments="comments" @load="loadComments"></app-comments>
  <div class="loader" v-if="loading"></div>
  <div v-if="alert !== 0" v-text="alert"></div>
</template>

<script>
import AppRezume from './components/AppRezume'
import axios from 'axios'
import AppComments from './components/AppComments.vue'
export default {
  components: { AppRezume, AppComments },
  data () {
    return {
      type: 'title',
      value: '',
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
    async createBlock () {
      const response = await fetch(
        'https://vuejs-with-http-default-rtdb.firebaseio.com/rezume.json',
        {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({
            typeBlock: this.type,
            valueBlock: this.value
          })
        }
      )
      const firebaseData = await response.json()
      this.blocks.push({
        typeBlock: this.type,
        valueBlock: this.value,
        id: firebaseData.name
      })
      this.type = 'title'
      this.value = ''
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

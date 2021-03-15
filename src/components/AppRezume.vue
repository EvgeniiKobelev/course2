<template>
  <div class="card card-w70">
    <div v-for="item in blocks" :key="item.id">
      <div class="center" v-if="item.typeBlock == 'title'">
        <h1>{{ item.valueBlock }}</h1>
      </div>

      <div class="avatar" v-if="item.typeBlock == 'avatar'">
        <img :src="item.valueBlock" />
      </div>
      <div v-if="item.typeBlock == 'subtitle'">
        <h2>{{ item.valueBlock }}</h2>
      </div>
      <div v-if="item.typeBlock == 'text'">
        <p>
          {{ item.valueBlock }}
        </p>
      </div>
    </div>
    <div v-if="alert !== 0" v-text="alert"></div>
    <div v-if="blocks === 0">
      <h3>Добавьте первый блок, чтобы увидеть результат</h3>
    </div>
  </div>
</template>
<script>
export default {
  props: ['blocks', 'alert']
}
</script>

<template>
  <div class="container">
    <p>
      <button class="btn primary" @click="$emit('load')">
        Загрузить комментарии
      </button>
    </p>
    <div class="card" v-for="comment in comments" :key="comment.id">
      <h2>Комментарий {{ comment.id }}</h2>
      <ul class="list">
        <li class="list-item">
          <div>
            <p>
              <strong>{{ comment.email }}</strong>
            </p>
            <small>{{ comment.body }}</small>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>
<script>
export default {
  emits: ['load'],
  props: ['comments']
}
</script>

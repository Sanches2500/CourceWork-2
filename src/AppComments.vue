<template>
  <div class="card">
    <h2>Комментарии</h2>
    <ul class="list">
      <li class="list-item" v-for="item in comments" :key="item.id">
        <div>
          <p><strong>{{ item.email }}</strong></p>
          <small>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Eligendi, reiciendis.</small>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: {
    comments: Array
  }
}
</script>

<style scoped>

</style>

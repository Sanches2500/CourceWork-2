<template>
  <div class="avatar">
    <img :src="url">
  </div>
</template>

<script>
export default {
  props: {
    url: {
      type: String,
      required: true
    }
  }
}
</script>

<style scoped>
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

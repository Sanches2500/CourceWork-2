<template>
  <button
    class="btn"
    :class="mod"
    @click="$emit('action')"
  >
    <slot></slot>
  </button>
</template>

<script>
export default {
  emits: ['action'],
  props: {
    mod: {
      type: String,
      validator (value) {
        return ['danger', 'primary'].indexOf(value) !== -1
      }
    }
  }
}
</script>


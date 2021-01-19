<template>
  <div v-for="item in componentsApp" :key="item.id">
    <component
      :url="item.componentType === 'avatar' ? item.componentContent : ''"
      :is="`app-${item.componentType}`"
    >{{ item.componentContent }}</component>
  </div>
</template>

<script>
import AppTitle from '@/AppTitle'
import AppSubtitle from '@/AppSubtitle'
import AppAvatar from '@/AppAvatar'
import AppText from '@/AppText'

export default {
  props: {
    componentsApp: Array,
  },
  data () {
    return {
      currentType: ''
    }
  },
  components: {
    AppTitle, AppSubtitle, AppAvatar, AppText
  }

}
</script>

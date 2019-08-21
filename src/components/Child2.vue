<template>
    <p>点击次数: {{count}}</p>
</template>
<script>
import Event from '../bus.js'

export default{
  name: 'Child2',
  data () {
    return {
      count: 0
    }
  },
  created () {
    Event.$on('update:count', () => {
      ++this.count
    })
  }
}
</script>
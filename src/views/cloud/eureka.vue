<template>
  <targetWindow :targetUrl="url"></targetWindow>
</template>

<script>
  import targetWindow from '@/components/Iframe'

  export default {
    name: "eureka",
    components: {targetWindow},
    data() {
      return {
        url: 'http://localhost:8761'
      }
    }
  }
</script>

<style scoped>

</style>

<template>
  <div>
    <span>{{ md }}</span>
    
  </div>
</template>

<script>
  export default {
    name: 'App',

    data() {
      return {
        
      }
      
    },

    mounted() {
      md = forge.md.md5.create()
      md.update('The quick brown fox jumps over the lazy dog')
    },

    methods: {

    },
  }
</script>
<style scoped>
  @import url(./assets/css/normalize.css);
</style>

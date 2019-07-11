<template>
  <div id="main-page">
    <center>
      <Input/>
      <Footer/>
    </center>
  </div>
</template>
<script>
  import Input from './Input.vue';
  import Footer from './Footer.vue';
  export default {
    components: {
      'Input':Input,
      'Footer':Footer
    },
    data() {
      return {

      }
    }
  }
</script>
<style scoped>
</style>

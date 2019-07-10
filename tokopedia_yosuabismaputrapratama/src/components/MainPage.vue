<template>
  <div id="main-page">
    <center>
      <Input/>
    </center>
  </div>
</template>
<script>
  import Input from './Input.vue';
  export default {
    components: {
      'Input':Input,
    },
    data() {
      return {

      }
    }
  }
</script>
<style scoped>
  #main-page{
    /* background-image: url("./../assets/tokopedia-banner-bg-new.png");
    background-position: center;
    background-repeat: no-repeat;
    background-size: cover;
    height: 100%; */
    margin-top: 40px;
  }
</style>

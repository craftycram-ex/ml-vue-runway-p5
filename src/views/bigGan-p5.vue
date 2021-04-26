<template>
  <Base64Image :imgData="imgData"/>
</template>

<script>
import Base64Image from '../components/base64Image.vue';
  export default {
    components: {
      Base64Image,
    },
    data() {
      return {
        imgData: '',
      };
    },
    mounted() {
      setInterval(() => {
        this.axios.get('http://10.10.1.37:8000/data').then((response) => {
        console.log(response.data)
        this.imgData = response.data.generated_output;
      });
      }, 250);
    },
  }
</script>

<style>

</style>
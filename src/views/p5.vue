<template>
  <vue-p5 v-on="this"></vue-p5>
</template>

<script>
import VueP5 from 'vue-p5';

export default {
  mounted() {
    this.$axios.get('localhost:8000').then((response) => {
      console.log(response.data)
    });
  },
  methods: {
    setup(sketch) {
      sketch.background('green');
      sketch.text('Hello p5!', 20, 20);
    }
  },
  render(h) {
    return h(VueP5, {on: this});
  }
};
</script>

<style>

</style>
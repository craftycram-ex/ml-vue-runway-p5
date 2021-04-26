<template>
  <vue-p5 v-if="loaded" class="p5" @setup="setup" @draw="draw" @mouseclicked="mouseclicked"></vue-p5>
</template>

<script>
  import VueP5 from 'vue-p5';
  export default {
    components: {
      "vue-p5": VueP5
    },
    data() {
      return {
        imgData: '',
        img: null,
        height: 400,
        width: 600,
        loaded: false,
      };
    },
    mounted() {
      this.axios.get('http://10.10.1.37:8000/data').then((response) => {
        console.log(response.data)
        if (response.data === null) return;
        if (response.data.generated_output !== null) {
          this.imgData = response.data.generated_output;
        } else {
          this.imgData = 'data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAAUAAAAFCAYAAACNbyblAAAAHElEQVQI12P4//8/w38GIAXDIBKE0DHxgljNBAAO9TXL0Y4OHwAAAABJRU5ErkJggg==';
        }
        this.loaded = true;
      });
    },
    methods: {
      setup(sketch) {
        this.width = sketch.displayWidth - 100;
        this.height = sketch.displayHeight - 100;
        sketch.createCanvas(this.width, this.height);
        this.img = sketch.loadImage(this.imgData);
      },
      draw(sketch) {
        sketch.background(0);
        sketch.image(this.img, 0, 0, this.width, this.height);

      },
      mouseclicked() {
        this.$emit('click-event');
      },
    }
  }
</script>

<style>

</style>
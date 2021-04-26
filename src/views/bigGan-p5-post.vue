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
      if (this.$route.query.type === undefined) {
        alert('please request type with ?type=')
        return;
      }
      this.axios.post('http://10.10.1.37:8000/query', { category: this.$route.query.type }).then((response) => {
        console.log(response.data)
        if (response.data === null) return;
        if (response.data.generated_output !== null) {
          this.imgData = response.data.generated_output;
        } else {
          this.imgData = 'data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAAUAAAAFCAYAAACNbyblAAAAHElEQVQI12P4//8/w38GIAXDIBKE0DHxgljNBAAO9TXL0Y4OHwAAAABJRU5ErkJggg==';
        }
        this.loaded = true;
      }).catch((err) => {
        const errMsg = err.response.data.error;
        if (errMsg.startsWith('Invalid argument: category')) {
          alert('invalid category')
          return;
        }
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
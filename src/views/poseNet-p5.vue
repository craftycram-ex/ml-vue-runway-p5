<template>
  <vue-p5 class="p5" @setup="setup" @draw="draw" @mouseclicked="mouseclicked"></vue-p5>
</template>

<script>
  import VueP5 from 'vue-p5';
  export default {
    components: {
      "vue-p5": VueP5
    },
    data() {
      return {
        posenet: {},
        ellipseSize: 10,
        height: 400,
        width: 600,
      };
    },
    mounted() {
      setInterval(() => {
        this.axios.get('http://localhost:8000/data').then((response) => {
        console.log(response.data)
        this.posenet = response.data;
      });
      }, 250);
    },
    methods: {
      setup(sketch) {
        this.width = sketch.displayWidth - 100;
        this.height = sketch.displayHeight - 100;
        sketch.createCanvas(this.width, this.height);
      },
      draw(sketch) {
        sketch.background(0);
        const humans = this.posenet.poses;
        humans.forEach((human) => {
          human.forEach((point, i) => {
            if (i == 0) {
              sketch.push();
              sketch.fill(255, 0, 0);
              sketch.ellipse(point[0] * this.width, point[1] * this.height, this.ellipseSize, this.ellipseSize);
              sketch.pop();
            } else {
              sketch.ellipse(point[0] * this.width, point[1] * this.height, this.ellipseSize, this.ellipseSize);
            }
          });
        });
      },
      mouseclicked() {
        this.$emit('click-event');
      }
    }
  }
</script>

<style>

</style>
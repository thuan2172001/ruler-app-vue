<template>
<div>
  <div class="slider">
    <div v-if="!showRuler">
      <!-- <p>You are using {{screenSize}} inch screen!</p> -->
      <vue-slider :style="slider" v-model="value"></vue-slider>
      <button @click="save" class="btn btn-success mb-3 mr-2">Save</button>
      <button class="btn btn-danger mb-3 ml-2">I don't have a card</button>
    </div>
    <div v-if="showRuler">
      <button v-if="showRuler" @click="rotate" class="btn btn-info mb-3 mr-2 ml-2">Rotate</button>
      <button v-if="showRuler" @click="measureScreen" class="btn btn-danger mb-3 ml-2">Measure screen size</button>
    </div>
  </div>
  <div class="image">
    <img v-if="!showRuler" v-bind:width="200 + value * 2" alt="Credit card" src="../assets/creditCard.jpg">
    <img v-if="showRuler" v-bind:style="directorClass" v-bind:width="(200 + standardValue * 2) * 30 / 8.56" alt="Credit card" src="../assets/ruler.jpg">
  </div>
</div>
</template>
<script>
import vueSlider from 'vue-slider-component';
import 'vue-slider-component/theme/default.css';
import 'bootstrap/dist/css/bootstrap.min.css';

export default {
  components: {
    vueSlider
  },
  data () {
    return {
      slider:{
        width: "80vw",
        height: "15px",
        marginLeft: "auto",
        marginRight: "auto",
        marginBottom: "20px"
      },
      directorClass: "transform: rotate(0deg)",
      value: 80,
      standardValue: 80,
      showRuler: false,
      screenSize: 15
    }
  },
  methods: {
    save: function () {
      this.standardValue = this.value
      this.showRuler = true
    },
    measureScreen: function () {
      this.showRuler = false
    },
    rotate: function () {
      if (this.directorClass == "transform: rotate(90deg) translateX(600px) translateY(500px)") 
        this.directorClass = "transform: rotate(0deg)"
      else this.directorClass = "transform: rotate(90deg) translateX(600px) translateY(500px)"
    }
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>

<template>
<div class="container sliderPage">
  <div v-if="!showRuler">
    <div v-if="!defaultValue">
      <h2>Your screen's default matrix:</h2>
      <h5>dpi: {{(200 + 2 * 80) / (rulerLengthDefault * 0.393700787)}} & dpi: {{(200 + 2 * 80) / rulerLengthDefault}} </h5>
      <p>Please measure your screen size </p>
    </div>
    <div v-else><h5>dpcm: {{dpcm}} & dpi: {{dpi}}</h5></div>
    <vue-slider :style="slider" v-model="value"></vue-slider>
  </div>
  <div class="imageSlider">
    <div class="image">
      <img v-if="!showRuler" v-bind:width="200 + value * 2" alt="Credit card" src="../assets/creditCard.jpg">
      <img id="ruler" v-if="showRuler" v-bind:style="directorClass" v-bind:width="(200 + value * 2) * rulerLengthDefault / 8.56" alt="Ruler" src="../assets/ruler10cm.jpg">
    </div>
  </div>
  <div v-if="!showRuler" class="mt-3">
    <button @click="save" class="btn btn-success mr-2">Save</button>
    <button @click="resetDefault" class="btn btn-danger ml-2">Reset</button>
  </div>
  <div v-else-if="showRuler" class="mt-3">
    <button @click="rotate" class="btn btn-info mr-2 ml-2">Rotate</button>
    <button @click="drag" class="btn btn-info mr-2 ml-2">Drag it</button>
    <button @click="measureScreen" class="btn btn-danger ml-2">Measure screen size</button>
  </div>
</div>
</template>
<script>
import vueSlider from 'vue-slider-component';
import 'vue-slider-component/theme/default.css';
import 'bootstrap/dist/css/bootstrap.min.css';
import rulerImage from '../assets/ruler10cm.jpg';

export default {
  components: {
    vueSlider
  },
  data () {
    return {
      slider:{
        width: "60vw",
        height: "15px",
        marginLeft: "auto",
        marginRight: "auto",
        marginBottom: "20px",
      },
      directorClass: "transform: rotate(0deg)",
      value: localStorage.getItem('value') || 80,
      defaultValue: localStorage.getItem('defaultValue'),
      rulerLengthDefault: 10,
      showRuler: false,
      windowRef: null,
    }
  },
  computed: {
    dpcm () {
      return (200 + 2 * this.value) / 10
    },
    dpi () {
      return (200 + 2 * this.value) / (10 * 0.393700787)
    }
  },
  methods: {
    save: function () {
      this.dpcm = (200 + 2 * this.value) / 10,
      this.dpi = (200 + 2 * this.value) / (10 * 0.393700787),
      this.showRuler = true
      this.defaultValue = true
      localStorage.setItem('value', this.value)
      localStorage.setItem('defaultValue', true)
    },
    measureScreen: function () {
      this.showRuler = false
      this.directorClass = "transform: rotate(0deg)"
    },
    rotate: function () {
      if (this.directorClass == "transform: rotate(90deg) translateX(150px) translateY(20vw)") 
        this.directorClass = "transform: rotate(0deg)"
      else this.directorClass = "transform: rotate(90deg) translateX(150px) translateY(20vw)"
    },
    resetDefault: function() {
      this.value = 80,
      localStorage.setItem('value', this.value),
      localStorage.removeItem('defaultValue')
    },  
    watch: {
      open(newOpen) {
        if(newOpen) {
          this.openPortal();
        } else {
          this.closePortal();
        }
      }
    },
    drag() {
      const width = (200 + this.value * 2) * this.rulerLengthDefault / 8.56
      this.windowRef = window.open("", "_blank", "width:800px");
      this.windowRef.document.write(`<img width=${width}px src=${rulerImage}>`)
    }
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .sliderPage {
    width: 80vw;
    height: 550px;
    padding: 10px;
    border: 2px solid black;
    overflow-x : scroll;
  }
  .imageSlider {
    height: 300px;
  }
</style>

<template>
<div class="container sliderPage">
  <div v-if="!wantMeasure && !defaultValue">
    <h2 v-if="!isDetect" class="mb-4">Click button to detect your screen!</h2>
    <h2 v-if="isDetect" class="mb-4"> Your screen's matrix: dpi = {{defaultDpi}} </h2>
    <button @click="getAutoDetectDpi" v-if="!isDetect" class="btn btn-primary mr-2">Detect my screen Dpi</button>
    <button @click="useDefault" v-if="isDetect" class="btn btn-success ml-2">Use default</button>
    <button @click="measureAgain" v-if="isDetect" class="btn btn-danger ml-2">Measure again</button>
  </div>
  <div v-if="wantMeasure || defaultValue">
    <div v-if="!showRuler">
      <div v-if="!defaultValue">
        <h2>Your screen's matrix: </h2>
        <h5>dpi: {{(200 + 2 * value) / (rulerLengthDefault * 0.393700787)}} & dpcm: {{(200 + 2 * value) / rulerLengthDefault}} </h5>
        <p>Please measure your screen size </p>
      </div>
      <div v-else>
        <h2>Your screen's matrix: </h2>
        <h5>dpcm: {{dpcm}} & dpi: {{dpi}}</h5>
      </div>
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
      <button @click="detectAgain" class="btn btn-primary mr-2">Detect again</button>
      <button @click="resetDefault" class="btn btn-danger mr-2">Reset</button>
    </div>
    <div v-else-if="showRuler" class="mt-3">
      <button @click="rotate" class="btn btn-info mr-2 ml-2">Rotate</button>
      <button @click="drag" class="btn btn-info mr-2 ml-2">Drag it</button>
      <button @click="measureScreen" class="btn btn-danger ml-2">Measure screen size</button>
    </div>
  </div>
  <div id='testdiv' style='height: 1in; left: -100%; position: absolute; top: -100%; width: 1in;'></div>
</div>
</template>

<script>
import vueSlider from 'vue-slider-component';
import 'bootstrap/dist/css/bootstrap.min.css';
import '../style/slider.css';
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
      defaultDpi: null,
      value: localStorage.getItem('value') || 80,
      defaultValue: localStorage.getItem('defaultValue'),
      rulerLengthDefault: 10,
      wantMeasure: false,
      showRuler: false,
      windowRef: null,
      isDetect: false,
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
    useDefault: function () {
      this.wantMeasure = true,
      this.showRuler = true,
      this.value = (this.defaultDpi * 10 * 0.393700787 - 200) / 2;
    },
    measureAgain: function () {
      this.wantMeasure = true
    },
    rotate: function () {
      if (this.directorClass == "transform: rotate(90deg) translateX(150px) translateY(20vw)") 
        this.directorClass = "transform: rotate(0deg)"
      else this.directorClass = "transform: rotate(90deg) translateX(150px) translateY(20vw)"
    },
    resetDefault: function() {
      this.value = (this.defaultDpi * 10 * 0.393700787 - 200) / 2;
      localStorage.setItem('value', this.value),
      localStorage.removeItem('defaultValue')
    },
    drag() {
      const width = (200 + this.value * 2) * this.rulerLengthDefault / 8.56
      this.windowRef = window.open("", "_blank", "width:800px");
      this.windowRef.document.write(`<img width=${width}px src=${rulerImage}>`)
    },
    getAutoDetectDpi() {
      const dpi_x = document.getElementById('testdiv').offsetWidth;
      this.isDetect = true,
      this.defaultDpi = dpi_x;
    },
    detectAgain () {
      this.defaultValue = null,
      this.isDetect = false,
      this.wantMeasure = false
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

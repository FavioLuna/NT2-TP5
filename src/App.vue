<template>
  <div id="app">
    <div v-if="!win">
      <IndexHeader :colorD="pickedColor" :hColor="hColor"/>

      <IndexNavigator :buttonMessage="buttonMessage" 
      :message="message"
      @count="restart($event)"
      
      />

      <div v-for="(colo, i) in colors" :key="i">
        <IndexSquare :color="colo" @onClick="clickSquare($event)"/>
      </div>
    </div>

    <div v-if="win">
      <IndexHeader :colorD="pickedColor" :hColor="pickedColor"/>

      <IndexNavigator :buttonMessage="buttonMessage" 
      :message="message"
      @count="restart($event)"
      
      />

      <div v-for="(colo, i) in colors" :key="i">
        <IndexSquare :color="colo" @onClick="clickSquare($event)"/>
      </div>
    </div>

  </div>
</template>

<script>

import IndexHeader from "./components/IndexHeader.vue";
import IndexNavigator from "./components/IndexNavigator.vue";
import IndexSquare from "./components/IndexSquare.vue";

export default {
  name: 'App',
  props: [],
  components: {
    IndexHeader,
    IndexNavigator,
    IndexSquare
  },
  mounted(){
    this.restart(this.colorCount)
  },  
  data(){
    return{
      colors:[],
      pickedColor:"",
      colorCount: 6,
      message: "",
      colorDisplay:"",
      hColor: "",
      buttonMessage: "New Colors!",
      win: false,
    }
  },
  methods:{
    restart(cCount){
      this.colors = this.createNewColors(cCount);
      this.pickedColor = this.colors[this.PickColor()];
      this.colorDisplay = this.pickedColor;
      this.hColor = "steelblue";
      this.message = "";
      this.buttonMessage = "New Colors!";
      this.win = false;
    },
    createNewColors(numbers){
      var arr = [];
      for (var i = 0; i < numbers; i++) {
        arr.push(this.createRandomStringColor());
      }
        return arr;
    },
    createRandomStringColor(){
      var newColor = "rgb(" + this.randomInt() + ", " + this.randomInt() + ", " + this.randomInt() + ")" ;
      return newColor;
    },
    randomInt(){
      return Math.floor(Math.random() * 256);
    },
    clickSquare(clickedColor){
      if (clickedColor === this.pickedColor) {
        this.message = "You Picked Right!";
        this.setAllColorsTo(this.pickedColor);
        this.buttonMessage = "Play Again!";
        this.win = true
      } else {
        this.message = "Try Again!";
        this.win = false
      }
    },
    setAllColorsTo(color) {
      this.colors.forEach(square => {
        square = color;
        console.log(square + "setAll")
      });
    },
    PickColor(){
      var quantity;
      if (this.isHard) {
        quantity = 6;
      } else {
        quantity = 3;
      }
      return Math.floor(Math.random() * quantity );
    },

  }
}
</script>

<style>
#app {
  background: #232323;
  margin: 0;
  font-family: "Montserrat", "Avenir";
}
h1 {
  font-weight: normal;
  line-height: 1.1;
  padding: 20px 0;
}
</style>

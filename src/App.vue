<template>
  <div id="app">
     <HeaderGame 
     v-bind:color-jugar='colorJugar'
     v-bind:gano='gano'
     ></HeaderGame>
      <br>
     <Buttons  v-bind:mensaje='mensaje' 
      v-bind:restart="restart"
       v-bind:dificultadEasy="dificultadEasy"
       v-bind:dificultadHard="dificultadHard"
       v-bind:is-Hard='isHard' >
    </Buttons>

    <Item v-for="(color, index) in colors"
            v-bind:color="color"
            v-bind:color-clikeado='colorclickeado'
            v-bind:index="index"
            v-bind:key="myKey(color)" 
            v-bind:colors='colors'
            v-bind:conocer-ganador="conocerGanador">
    </Item>

    </div>
</template>

<script>
import HeaderGame from './components/HeaderGame.vue'
import Buttons from './components/Buttons.vue'
import Item from './components/Item.vue'



export default {
  name: 'App',
  data () {
    return {
      colorJugar: {
           r: 0,
           g: 0,
           b: 0
       },
       colors: [],
       mensaje:"",
       colorclickeado:"",
       isHard:true,
       colorCount:6,
       gano:false

    }
  },
   
   mounted () {   
      this.restart()   
    },

  methods: {
        conocerGanador(colorcuadrado, cuadradoIndex){
            if(JSON.stringify(colorcuadrado) === JSON.stringify(this.colorJugar)){
                this.mensaje="Ganaste!";
                this.gano=true;
                for (let index = 0; index < this.colors.length; index++) {
                    Object.assign(this.colors[index], colorcuadrado);
                  }
            }else{
                this.mensaje="Intenta de nuevo!";
                
                this.colors[cuadradoIndex].r = 35;
                this.colors[cuadradoIndex].g = 35;
                this.colors[cuadradoIndex].b = 35;
            }
        },
         
        myKey(color){
            const randomNumber = Math.random() * 100000;
            return '' + randomNumber + color.r +color.g +color.b;
        },

        dificultadEasy(){
            if (this.isHard) {
                this.isHard = false;
                this.colorCount = 3;
                this.restart();
            }
        },
        dificultadHard(){
            if (!this.isHard) {
                this.isHard = true;
                this.colorCount = 6;
                this.restart();
            }
        },
        restart(){
            this.colors=this.createNewColors(this.colorCount);
            let pickedColor = this.colors[this.pickColor()];
            this.colorJugar.r = pickedColor.r;
            this.colorJugar.g = pickedColor.g;
            this.colorJugar.b = pickedColor.b;
            this.mensaje="";
            this.gano=false;

        },
        createNewColors(numbers){
            let arr = [];
            for (var i = 0; i < numbers; i++) {
                arr.push(this.createRandomStringColor());
            }
            return arr;
        },
        createRandomStringColor(){
            return {
                r: this.randomInt(),
                g: this.randomInt(), 
                b: this.randomInt()
            };
        }, 
        randomInt(){
            return Math.floor(Math.random() * 256);
        },
        pickColor(){
            let quantity = 3;
            if (this.isHard) {
                quantity = 6;
            }
            return Math.floor(Math.random() * quantity );
        },        

    },

  components: {
    HeaderGame,
    Buttons,
    Item
  }


}
</script>

<style>
body {
	background: #232323;
	margin: 0;
	font-family: "Montserrat", "Avenir";
}

h1 {
	font-weight: normal;
	line-height: 1.1;
	padding: 20px 0;

}
#navigator {

	background: #ffffff;
	height: 30px;
	text-align: center;
	margin: 0;
	margin-top: -30px;

}
#header {
	transition: all 0.3s;
	background: steelblue;
	text-transform: uppercase;
	text-align: center;
	margin: 0;
	color: white;
	
}
#color_display {
	font-size: 200%;
}


.square {
	width: 30%;
	background: blue;
	padding-bottom: 30%;
	float: left;
	margin: 1.66%;
	border-radius: 10%;
	transition: background 0.8s;
	-webkit-transition: background 0.8s;
	-moz-transition: background 0.8s;

}
#container {
	margin: 20px auto;
	max-width: 600px;
}
#message {
	color: #0a0404;
	display: inline-block;
	width: 20%;
}

#messageBox {
	
}
.selected {
	background-color: steelblue;
	color: white;
}
button {
	border: none;
	background-color: white;
	font-family: "Montserrat", "Avenir";
	text-transform: uppercase;
	height: 100%;
	font-weight: 700;
	letter-spacing: 1px;
	color: steelblue;
	transition: all 0.3s;
	outline: none;
}
button:hover {
	color: white;
	background-color: steelblue;
}
</style>

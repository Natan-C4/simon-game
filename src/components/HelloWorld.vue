<template>
  <div class="wrapper">
		<h1>Simon Says</h1>
			<div class="game-board">
				<div class="simon">
					<ul>
						<li class="tile red" v-bind:style="{background: topRightRed}" @click="redClick"></li>
						<li class="tile blue" v-bind:style="{background : topLeftBlue}" @click="blueClick"></li>
						<li class="tile yellow" v-bind:style="{background : bottomLeftYellow}" @click="yellowClick" ></li>
						<li class="tile green" v-bind:style="{background:bottomRightGreen}" @click="greenClick"></li>
					</ul>
				</div>
			</div>
			<div class="game-info">
				<h2>Round: {{turnCounter}}</h2>
				<button  @click="startGame">Start</button>
			
			</div>
			<div class="game-options">
				<h2>Game Options:</h2>
				<input type="radio" v-model="mode" value="1500" checked>Easy<br>
				<input type="radio" v-model="mode" value="1000" >Normal<br>
				<input type="radio" v-model="mode" value="400" >Hard<br>
			</div>
		<div data-action="sound"></div>
	
	</div>
</template>

<script>
export default {

  

  name: 'HelloWorld',
  data: () => ({
    order: [],
    playerOrder:[],
    flash:null,
    strict : true,
    win:null,
    turn:null,
    good:null,
    compTurn:null,
    intervalID:null,
    noise:true,
    on:true,
    turnCounter:null,
    topLeftBlue:'darkblue',
    topRightRed: 'darkred',
    bottomLeftYellow:'goldenrod',
    bottomRightGreen:'darkgreen',
    mode: 1500,
    audioRed:new Audio('/audio/1.mp3'),
    audioBlue:new Audio('/audio/2.mp3'),
    audioGreen:new Audio('/audio/3.mp3'),
    audioYellow:new Audio('/audio/4.mp3'),

  }),
  methods:{
   
    startGame(){
      if(this.on || this.win){

        this.play()
      }

    },
    play(){
      this.win = false
      this.on = true
      this.order = []
      this.playerOrder = []
      this.flash = 0
      this.intervalID = 0
      this.turn = 1
      this.turnCounter = 1
      this.good = true
      for(let i = 0; i < 20 ; i++){
        this.order.push(Math.floor(Math.random() * 4) + 1)
      }
      this.compTurn = true

      this.intervalID = setInterval(this.gameTurn, this.mode)
     
      
    },
    clearColor(){
      this.topLeftBlue = 'darkblue'
      this.topRightRed = 'darkred'
      this.bottomLeftYellow = 'goldenrod'
      this.bottomRightGreen = 'darkgreen'
    },

    gameTurn(){
      this.on = false

      if (this.flash == this.turn){
        clearInterval(this.intervalID)
        this.compTurn = false
        this.clearColor()
        this.on = true
      }

      if(this.compTurn){
        this.clearColor()
        setTimeout(() => {
          if(this.order[this.flash] == 1){this.one()}
          if(this.order[this.flash] == 2){this.two()}
          if(this.order[this.flash] == 3){this.three()}
          if(this.order[this.flash] == 4){this.four()}
          this.flash++
        } , 200)
      }


      
      
    },
    
    one(){
      if(this.noise){
        this.audioBlue.play()
      }
      this.noise = true
      this.topLeftBlue = 'lightskyblue'
    },
    two(){
       if(this.noise){
        this.audioRed.play()
      }
      this.noise = true
      this.topRightRed = 'tomato'
    },
    three(){
       if(this.noise){
        this.audioYellow.play()
      }
      this.noise = true
      this.bottomLeftYellow = 'yellow'
    },
    four(){
       if(this.noise){
        this.audioGreen.play()
      }
      this.noise = true
      this.bottomRightGreen = 'lightgreen'
    },
    winGame(){
      this.flashColor()
      this.turnCounter = 'WIN'
      this.win = true
    },
    flashColor(){
      this.topLeftBlue = 'lightskyblue'
      this.topRightRed + 'tomato'
      this.bottomLeftYellow = 'yellow'
      this.bottomRightGreen = 'lightgreen'
    },
    check(){
      if(this.playerOrder[this.playerOrder.length -1] !== this.order[this.playerOrder.length - 1]){
        this.good = false
         
        }
        if(this.playerOrder.length == 20 && this.good){
          this.winGame()
        }
        if(this.good == false){
          this.flashColor()
          this.turnCounter = 'NO'
          setTimeout(() => {
            this.turnCounter = this.turn
            this.clearColor()

            if(this.strict){
              this.play()
            }else{
              this.compTurn = true
              this.flash = 0 
              this.playerOrder = []
              this.good = true
              this.intervalID = setInterval(this.gameTurn, this.mode)

            }
          }, 800)
          this.noise = false
        }
        if(this.turn == this.playerOrder.length && this.good && !this.win){
          this.turn++
          this.playerOrder = []
          this.compTurn = true
          this.flash = 0
          this.turnCounter = this.turn
          this.intervalID = setInterval(this.gameTurn, this.mode)
        }
      
    },
    blueClick(){
      if(this.on){
        this.playerOrder.push(1)
        this.check()
        this.one()
        if(!this.win){
          setTimeout(()=>{this.clearColor()},300)
          }

      }
    },
    redClick(){
      if(this.on){
        this.playerOrder.push(2)
        this.check()
        this.two()
        if(!this.win){
        setTimeout(()=>{this.clearColor()},300)
        }

      }
    },
    yellowClick(){
      if(this.on){
        this.playerOrder.push(3)
        this.check()
        this.three()
        if(!this.win){
        setTimeout(()=>{this.clearColor()},300)
        }

      }
    },
    greenClick(){
      if(this.on){
        this.playerOrder.push(4)
        this.check()
        this.four()
        if(!this.win){
        setTimeout(()=>{this.clearColor()},300)
        }

      }
    },
    
    
  },

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
body {
	font-family: Arial, serif;
	color:black;
	-webkit-user-select: none; /* Chrome/Safari */        
	-moz-user-select: none; /* Firefox */
	-ms-user-select: none; /* IE10+ */
	-o-user-select: none;
	user-select: none;
}

h1 {
	margin: 1em 0 2em;
	text-align: center;
}

ul {
	list-style: none;
}

ul, li {
	padding: 0;
	margin: 0;
}

p[data-action="lose"] {
	display: none;
}

.active {
	opacity: 1 !important;
}

.clearfix {
	width: 100%;
	clear: both;
}

.wrapper {
	width: 540px;
	margin: 0 auto;
}
.container {
	width: 305px;
}

.simon {
	background: #fff;
	position: relative;
	float: left;
	margin-right: 3em;	
	width: 302px;
	height: 295px;
	-webkit-border-radius: 150px 150px 150px 150px;
	border-radius: 150px 150px 150px 150px;
    -moz-box-shadow: 2px 1px 12px #aaa;
    -webkit-box-shadow: 2px 1px 12px #aaa;
    -o-box-shadow: 2px 1px 12px #aaa;
    box-shadow: 2px 1px 12px #aaa;
}

.tile {
	opacity: 0.6;
	-webkit-transition: opacity 250ms ease;
	-moz-transition: opacity 250ms ease;
	-ms-transition: opacity 250ms ease;
	-o-transition: opacity 250ms ease;
	transition: opacity 250ms ease;
}

.tile.lit {
	opacity: 1;
}

.red, .blue, .yellow, .green {
	height: 290px;
	-webkit-border-radius: 150px 150px 150px 150px;
	border-radius: 150px 150px 150px 150px;
	position: absolute;
	text-indent: 10000px;
}

.red:hover, .blue:hover, .yellow:hover, .green:hover {
	border: 2px solid black
}

.red {
	
	clip: rect(0px, 300px, 150px, 150px);
	width: 296px;
}

.blue {
	
	clip: rect(0px, 150px, 150px, 0px);
	width: 300px;
}

.yellow {
	
	clip: rect(150px, 150px, 300px, 0px);
	width: 300px;
}

.green {
	
	clip: rect(150px,300px, 300px, 150px);
	width: 296px;
}

.game-info {
	margin-top: 90px;
}

.game-info button {
	width: 5em;
	box-sizing: border-box;
	font-size: 1.4em;
	-webkit-border-radius: 10px 10px 10px 10px;
	border-radius: 10px 10px 10px 10px;
	background: #6DABE8;
	border: none;
	padding: 0.3em 0.6em;
}

.game-info button:hover {
	background: #78BCFF;
}

.game-options h2 {
	margin-top: 30px;
	margin-bottom: 0;
}

.game-options input[type="radio"] {
	margin-right: 10px;
}

.hoverable:hover {
	cursor: pointer;
}

footer {
	position: absolute;
	bottom: 20px;
	width: 540px;
	clear: both;
	text-align: center;
}
</style>

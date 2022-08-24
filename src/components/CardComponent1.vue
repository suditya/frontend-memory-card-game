<template>
<div>
<header>
    <h1>MEMORY CARD GAME</h1>
</header>
<div class="game-container">
    <div class="d-flex flex-row justify-content-center py-3">
    <div class="turns p-3"><span class="btn btn-info">Turns : <span class="badge" :class="finish ? 'badge-success' : 'badge-danger'">{{turns}}</span> </span></div>
    <div class="totalTime p-3"><span class="btn btn-info">Total Time : <span class="badge" :class="finish ? 'badge-success' : 'badge-danger'">{{min}} : {{sec}}</span></span></div>
</div>
  <div class="row boxing">
        <div class="col-md-4 col-lg-5 mx-auto">
             <div class="row justify-content-md-center">
                    <div v-for="card in memoryCards" :key="card" class="col-auto mb-3 flip-container" :class="{ 'flipped': card.isFlipped, 'matched' : card.isMatched , 'shake' : animated}" @click="flipCard(card)" @click.prevent="playSound('http://soundbible.com/mp3/Elevator Ding-SoundBible.com-685385892.mp3')">
                    <div class="memorycard">
                        <div class="front border rounded shadow"></div>
                        <!-- {{card.img}} -->
                        <div class="back rounded border"><img height="150px" width="90%" alt="xy"  :src="card.img"></div>
                    </div>
                 </div>
            </div>
        </div>
    </div>
    <div class="restart p-3"><button class="btn btn-info" @click="reset" :disabled="!start">Restart</button></div>
    
</div>
<div id="win-screen" class="">
    <h3>🎯 Nailed It! 🎯</h3>
    <div class="score click-count">Total Clicks: 0</div>
    <div class="score low-score">Low Score:</div>
    <a id="replay-button">Play Again?</a>
  </div>
</div>
</template>

<script>
import '../assets/style.css'

import _ from 'lodash';
export default {
    name:'CardComponent1',
    data() {
        return {
            cards: [
                {
                    name: 'Apple',
                    img: 'https://png.pngtree.com/element_our/png/20181014/attitude-emoji-icon-design-vector-png_124998.jpg',

                },
                {
                    name: 'Banana',
                    img: 'https://png.pngtree.com/png-vector/20190411/ourmid/pngtree-vector-surprise-emoji-icon-png-image_924612.jpg',
 
                },
                {
                    name: 'Orange',
                    img: 'https://png.pngtree.com/element_our/20190602/ourmid/pngtree-color-smiley-face-expression-image_1410314.jpg',

                },
                {
                    name: 'Pineapple',
                    img: 'https://png.pngtree.com/png-clipart/20220614/ourmid/pngtree-emoji-3d-rendering-yellow-stereo-png-image_4991238.png',

                },
                {
                    name: 'Strawberry',
                    img: 'https://png.pngtree.com/png-vector/20210114/ourmid/pngtree-3d-wow-emoticon-emoji-png-image_2744064.jpg',

                },
                {
                    name: 'watermelon',
                    img: 'https://png.pngtree.com/element_our/png/20181130/calm-emoji-vector-icon-png_255595.jpg',

                },
            ],
            memoryCards: [],
            flippedCards: [],
            finish: false,
            start: false,
            turns: 0,
            totalTime: {
                minutes: 0,
                seconds: 0,
            },
            animated: false,
        }
    },
    created(){
        this.reset();
    },
    methods:{
    playSound (sound) {
      if(sound) {
        let audio = new Audio(sound);
        audio.play();
      }
    },
    flipCard(card){

        if(card.isMatched || card.isFlipped || this.flippedCards.length === 2)
                return;
        if(!this.start)
        {
            this._startGame();
        }
        card.isFlipped = true;

        if(this.flippedCards.length < 2)
            this.flippedCards.push(card);
        if(this.flippedCards.length === 2)    
        {
            this._match(card);
            
        }

    },
    _match(card)
    {
        console.log(card);
        this.turns++;
        if(this.memoryCards.every(card => card.isMatched === true)){
        clearInterval(this.interval);
        this.finish = true;
        }
        if(this.flippedCards[0].name === this.flippedCards[1].name){
            setTimeout(() => {
            this.flippedCards.forEach(card => card.isMatched = true);
            this.flippedCards = [];

            //All cards matched ?
            if(this.memoryCards.every(card => card.isMatched === true)){
                this.finish = true;
            }

            }, 400);
        }
        else{
            setTimeout(()=>
            {
                this.animated=true;
            },300);
            setTimeout(() => {
                this.flippedCards.forEach((card) => {card.isFlipped = false});
                this.flippedCards = [];
                this.animated = false;
            }, 700);
        }
    },
    _startGame()
    {
        this._tick();
        this.interval = setInterval(this._tick,1000);
        this.start = true;
    },

    _tick(){
        if(this.finish==true) return ;
        if(this.totalTime.seconds !== 59){
            this.totalTime.seconds++;
            return
        }

        this.totalTime.minutes++;
        this.totalTime.seconds = 0;
    },
    reset()
    {
        clearInterval(this.interval);

        this.cards.forEach((card) => {
            this.$set(card,'isFlipped',false);
            this.$set(card,'isMatched',false);
        });

        setTimeout(() => {  
            this.memoryCards = [];
            this.memoryCards = _.shuffle(this.memoryCards.concat(_.cloneDeep(this.cards), _.cloneDeep(this.cards)));
            this.totalTime.minutes = 0;
            this.totalTime.seconds = 0;
            this.start = false;
            this.finish = false;
            this.turns = 0;
            this.flippedCards = [];
            
            }, 800);
        
    },
    },
    computed:{
        sec(){
            if(this.totalTime.seconds < 10){
                return '0'+this.totalTime.seconds;
            }
            return this.totalTime.seconds;
        },
        min(){
            if(this.totalTime.minutes < 10){
                return '0'+this.totalTime.minutes;
            }
            return this.totalTime.minutes;
        }
    }
}

</script>

<style>
    *
    {
        box-sizing: border-box;
    }
    body 
    {
        margin: 0;
        color: #4d4d4d;
        font-family: Open Sans, sans-serif;
        letter-spacing: 1px;
        font-weight: 300;
        font-size: 15px;
    }
    body 
    {
        margin: 0;
        color: #4d4d4d;
        font-family: Open Sans, sans-serif;
        letter-spacing: 1px;
        font-weight: 300;
        font-size: 15px;
    }
    .memorycard
    {
        box-sizing: border-box;
        width: 150px;
        text-align: center;
    }
    .flip-container 
    {
        -webkit-perspective: 5000px;
        -moz-perspective: 5000px;
        -o-perspective: 5000px;
        perspective: 5000px;
        min-height: 120px;
        cursor: pointer;
    }
    .front,.back 
    {
        -webkit-backface-visibility: hidden;
        -moz-backface-visibility: hidden;
        -o-backface-visibility: hidden;
        backface-visibility: hidden;
        -webkit-transition: 0.6s;
        -webkit-transform-style: preserve-3d;
        -moz-transition: 0.6s;
        -moz-transform-style: preserve-3d;
        -o-transition: 0.6s;
        -o-transform-style: preserve-3d;
        -ms-transition: 0.6s;
        -ms-transform-style: preserve-3d;
        transition: 0.6s;
        transform-style: preserve-3d;
        top: 0;
        left: 0;
        width: 100%;
        /* background-color: lightpink; */
    }
    .front
    {
        width: 150px;
        height: 150px;
        background-color:aqua;
    }
    .back {
        -webkit-transform: rotateY(-180deg);
        -moz-transform: rotateY(-180deg);
        -o-transform: rotateY(-180deg);
        -ms-transform: rotateY(-180deg);
        transform: rotateY(-180deg);
        position: absolute;
        /* background-color: lightpink; */
    }
    .flip-container.flipped .back {
        -webkit-transform: rotateY(0deg);
        -moz-transform: rotateY(0deg);
        -o-transform: rotateY(0deg);
        -ms-transform: rotateY(0deg);
        transform: rotateY(0deg);
    }
    .flip-container.flipped .front {
        -webkit-transform: rotateY(180deg);
        -moz-transform: rotateY(180deg);
        -o-transform: rotateY(180deg);
        -ms-transform: rotateY(180deg);
        transform: rotateY(180deg);
    }
    .matched{
        opacity: 0.9;
    }
    .light-blue
    {
        background-color: #b6e6ff;
    }
    .restart
    {
        display:flex;
        justify-content: center;
    }
    header {
        background-color: lightpink;
        padding: 0px 8px 0px;
        color: white;
        height: 20%;
        /* height: 100vh; */
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
    }
    .shake {
        animation: shake 0.82s cubic-bezier(.36,.07,.19,.97) both;
        transform: translate3d(0, 0, 0);
    }
    @keyframes shake {
        10%, 90% {
            transform: translate3d(-1px, 0, 0);
        }
        20%, 80% {
            transform: translate3d(2px, 0, 0);
        }
        30%, 50%, 70% {
            transform: translate3d(-4px, 0, 0);
        }
        40%, 60% {
            transform: translate3d(4px, 0, 0);
        }
    }
    #win-screen {
    height: 100%;
    width: 100%;
    margin: 0;
    background-color: rgba(230,230,250, 0.95);
    position: absolute;
    left: 0;
    top: 0;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    visibility: hidden;
    opacity: 0;
    }
    #win-screen.visible {
    visibility: visible;
    opacity: 1;
    transition: all 0.8s;
    }

    #replay-button {
    cursor: pointer;
    padding: 15px;
    margin: 10px;
    background-color: #f0f0ff;
    border: 1px solid white;
    border-radius: 5px;
    box-shadow: 0px 0px 3px white;
    }

    #replay-button:hover {
    background-color: #f8f8ff;
    border: 1px solid white;
    box-shadow: 0px 0px 5px 1px white;
    }
</style>
<template>
    <section id="game-container" class="">
    <section id="game">
    <div id="game-card-1" class="game-card">
      <figure class="front"></figure>
      <figure class="back"></figure>
    </div>
    <div id="game-card-2" class="game-card">
      <figure class="front"></figure>
      <figure class="back"></figure>
    </div>
    <div id="game-card-3" class="game-card">
      <figure class="front"></figure>
      <figure class="back"></figure>
    </div>
    <div id="game-card-4" class="game-card">
      <figure class="front"></figure>
      <figure class="back"></figure>
    </div>
    <div id="game-card-5" class="game-card">
      <figure class="front"></figure>
      <figure class="back"></figure>
    </div>
    <div id="game-card-6" class="game-card">
      <figure class="front"></figure>
      <figure class="back"></figure>
    </div>
    <div id="game-card-7" class="game-card">
      <figure class="front"></figure>
      <figure class="back"></figure>
    </div>
    <div id="game-card-8" class="game-card">
      <figure class="front"></figure>
      <figure class="back"></figure>
    </div>
    <div id="game-card-9" class="game-card">
      <figure class="front"></figure>
      <figure class="back"></figure>
    </div>
    <div id="game-card-10" class="game-card">
      <figure class="front"></figure>
      <figure class="back"></figure>
    </div>
    <div id="game-card-11" class="game-card">
      <figure class="front"></figure>
      <figure class="back"></figure>
    </div>
    <div id="game-card-12" class="game-card">
      <figure class="front"></figure>
      <figure class="back"></figure>
    </div>
  </section>
  <section id="score-board" class="">
    <div class="score low-score">Low Score: N/A</div>
    <div class="score click-count">Total Clicks: 0</div>
  </section>
  <div id="win-screen" class="">
    <h3>🎯 Nailed It! 🎯</h3>
    <div class="score click-count">Total Clicks: 0</div>
    <div class="score low-score">Low Score:</div>
    <a id="replay-button">Play Again?</a>
  </div>
</section>
</template>
<script>


export default {
    name:'CardComponent',
    data()
    {
        return {
        possibleCardFaces : ["&#x1F602;", "&#x1F60E;", "&#x1F60D;", "&#x1F61C;", "&#x1F643;", "&#x1F913;", "&#x1F602;", "&#x1F60E;", "&#x1F60D;", "&#x1F61C;", "&#x1F643;", "&#x1F913;"],
        lowScore : localStorage.getItem("lowScore"),
        score : 0,
        flippedCards : [],
        matchedCards : [],
        locked : false,
        flipTimeout : 700
        }
    },
    methods: {
       
        getRandomIndex(length) {
            return Math.floor(Math.random() * length);
        }
        ,
        getRandomFace(randomIndex) {
            let face;
            randomIndex = this.getRandomIndex(this.possibleCardFaces.length);
            face = this.possibleCardFaces[randomIndex];
            this.possibleCardFaces.splice(randomIndex, 1);
            console.log(face);
            return face;
        }



    }
   


}
</script>

<style scoped>
    body {
    margin: 0;
    color: #4d4d4d;
    font-family: Open Sans, sans-serif;
    letter-spacing: 1px;
    font-weight: 300;
    font-size: 15px;
    }

    h1 {
    color: white;
    font-family: Comfortaa, cursive;
    letter-spacing: 3px;
    font-size: 42px;
    text-align: center;
    margin: 0;
    }

    h2 {
    font-size: 80px;
    margin: 5px 0 0;
    }

    h3 {
    font-size: 28px;
    font-weight: 300;
    margin: 0;
    }

    p {
    margin: 0;
    }

    a {
    color: inherit;
    }

    header {
    background-color: lightpink;
    padding: 0px 8px 0px;
    height: 100vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    }

    #new-game-button {
    text-decoration: none;
    color: white;
    margin-top: 40px;
    padding: 12px 15px;
    border: 1px solid;
    border-radius: 5px;
    box-shadow: 0px 0px 3px white;
    }

    #new-game-button:hover {
    background-color: white;
    color: lightpink;
    margin-top: 40px;
    padding: 12px 15px;
    border: 1px solid white;
    border-radius: 5px;
    box-shadow: 0px 0px 5px 2px white;
    }

    #game-container {
    position: relative;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 50px 8px 30px;
    }

    #game-container.hidden {
    display: none;
    }

    #game {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    align-items: center;
    max-width: 900px;
    perspective: 800px;
    }

    .game-card {
    height: 180px;
    width: 180px;
    margin: 5px;
    cursor: pointer;
    position: relative;
    transform-style: preserve-3d;
    transition: transform .65s;
    box-shadow: 0px 2px 4px gray;
    }

    .game-card figure {
    margin: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
    }

    .game-card .front {
    background-color: #b6e6ff;
    }

    .game-card .back {
    background-color: lightpink;
    transform: rotateY(180deg);
    }

    @media (max-width: 800px) {
    .game-card {
        height: 150px;
        width: 150px;
    }
    }

    .game-card.flipped {
    transform: rotateY(180deg);
    }

    #score-board {
    margin: 30px 0 0;
    display: flex;
    }

    .score {
    display: inline-block;
    padding: 0 20px 0;
    }

    #score-board.hidden {
    visibility: hidden;
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

    #win-screen>* {
    line-height: 0;
    margin: 25px;
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

    em {
    font-size: 14px;
    }

    footer {
    background-color: lightpink;
    font-size: 13px;
    padding: 12px;
    width: 100%;
    text-align: center;
    box-sizing: border-box;
    }

    footer.hidden {
    display: none;
    }

    footer a:hover {
    color: #808080;
    }

</style>

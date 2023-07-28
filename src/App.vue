<template>
	<div id="app">
		<div class="wrapper clearfix">

            <players
                v-bind:isWinner="isWinner"
                v-bind:scoresPlayer="scoresPlayer"
                v-bind:activePlayer="activePlayer"
                v-bind:currentScore="currentScore"
            />
            
            <controls
                v-bind:isPlaying="isPlaying"
                v-bind:finalScore="finalScore"
                v-on:handleFinalScore="handleFinalScore"
                v-on:handleNewGame="handleNewGame"
                v-on:handleRoleDice="handleRoleDice"
                v-on:handleHoldScore="handleHoldScore"
            />
            
            <dices
                v-bind:dices="dices"
            />

            <popup-rule
                v-on:handleConfirm="handleConfirm"
                v-bind:isOpenPopup="isOpenPopup"
            />
        </div>
	</div>
</template>

<script>
import Players from './components/Players.vue';
import Controls from './components/Controls.vue';
import Dices from './components/Dices.vue';
import PopupRule from './components/PopupRule.vue';
export default {
	name: 'app',
	data () {
		return {
			scoresPlayer: [0, 0],
            activePlayer: 0,
            dices: [6, 6],
            currentScore: 0,
            isPlaying: false,
            isOpenPopup: false,
            finalScore: 100
		}
	},
	components: {
        Players,
        Controls,
        Dices,
        PopupRule
	},
    methods: {
        handleNewGame(){
            this.isOpenPopup = true;
        },
        handleConfirm(){
            this.isPlaying = true;
            this.isOpenPopup = false;
            this.activePlayer =0;
            this.scoresPlayer = [0, 0];
            this.currentScore = 0;
            this.dices = [1, 1];
        },
        nextPlayer(){
            this.activePlayer = this.activePlayer === 0 ? 1 : 0;
            this.currentScore = 0;
        },
        handleRoleDice(){
            if(this.isPlaying){
                var dice1 = Math.floor(Math.random() * 6) +1;
                var dice2 = Math.floor(Math.random() * 6) +1;
                this.dices = [dice1, dice2];

                if(dice1 === 1 || dice2 === 1){
                    let activePlayer = this.activePlayer;
                    setTimeout(function(){
                        alert(`Người chơi ${activePlayer + 1} đã quay trúng số 1`);
                    }, 15);
                    this.nextPlayer();
                }
                else{
                    this.currentScore = this.currentScore + dice1 + dice2;
                }
            }
            else
                alert("Vui lòng ấn nút New Game");
        },
        handleHoldScore(){
            if(this.isPlaying){
                let { scoresPlayer, activePlayer, currentScore } = this;
                let scoreOld = scoresPlayer[activePlayer];
                let cloneScore = [...scoresPlayer];
                cloneScore[activePlayer] = scoreOld + currentScore;
                this.scoresPlayer = cloneScore;
                if(!this.isWinner)
                    this.nextPlayer();
            }
            else
                alert("Vui lòng ấn nút New Game");
        },
        handleFinalScore(e){
            var number = parseInt(e.target.value);
            if(isNaN(number)){
                this.finalScore =''
            }
            else
                this.finalScore = number;
        }
    },
    computed: {
        isWinner(){
            let {scoresPlayer, finalScore} = this;

            if(scoresPlayer[0] >= finalScore || scoresPlayer[1] >= finalScore){
                this.isPlaying = false;
                return true;
            }
            return false;
        }
    }
}
</script>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    
}

.clearfix::after {
    content: "";
    display: table;
    clear: both;
}

body {
    background-image: linear-gradient(rgba(62, 20, 20, 0.4), rgba(62, 20, 20, 0.4)), url('/public/assets/back.jpg');
    background-size: cover;
    background-position: center;
    font-family: Lato;
    font-weight: 300;
    position: relative;
    height: 100vh;
    color: #555;
}

.wrapper {
    width: 1000px;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: #fff;
    box-shadow: 0px 10px 50px rgba(0, 0, 0, 0.3);
    overflow: hidden;
}


</style>

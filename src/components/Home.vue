<template>
    <div class="container my-3">
        <h1>Rock, Paper, Scissors</h1>
    </div>
    <div class="container" v-if="!showResult">
        <div class="btn-group">
            <button class="btn btn-danger" @click="play('rock')">
                <i class="fa-solid fa-hand-back-fist fa-2xl py-5"></i>
                <div class="card" style="width: 18rem;">
                    <div class="card-body">
                        <p class="card-text text-danger">Rock</p>
                    </div>
                </div>
            </button>
            <button class="btn btn-primary" @click="play('paper')">
                <i class="fa-solid fa-note-sticky fa-2xl py-5"></i>
                <div class="card" style="width: 18rem;">
                    <div class="card-body">
                        <p class="card-text text-primary">Paper</p>
                    </div>
                </div></button>
            <button class="btn btn-info" @click="play('scissors')">
                <i class="fa-solid fa-scissors fa-2xl py-5 text-white"></i>
                <div class="card" style="width: 18rem;">
                    <div class="card-body">
                        <p class="card-text text-info">Scissors</p>
                    </div>
                </div>
            </button>
        </div>
        <div class="row mt-4">
            <h1> Win : {{winCount}} Draw : {{drawCount}} Loss : {{lossCount}} </h1>
        </div>
        <div class="row mt-4">
            <h1>Win Percentage : {{calcualtePercentage}}%</h1>
        </div>
    </div>
    <div class="container" v-if="showResult">
        <h1>You Picked {{userChoice}}</h1>
        <h1>AI Picked {{aiChoice}}</h1>
        <h1>You {{result}} !</h1>
        <button class="btn btn-primary" @click="restart()">Restart</button>
    </div>
</template>

<script>
export default {
    data() {
        return {
            rules : {
                rock : {
                    rock : 'draw',
                    paper : 'loss',
                    scissors : 'win',
                },
                paper : {
                    rock : 'win',
                    paper : 'draw',
                    scissors : 'loss',
                },
                scissors : {
                    rock : 'loss',
                    paper : 'win',
                    scissors : 'draw',
                }
            },
            choices : ['rock', 'paper', 'scissors'],
            userChoice : '',
            aiChoice : '',
            result: '',
            showResult : false,
            winCount : '',
            drawCount : '',
            lossCount : '',
            total : 0,
        }
    },
    methods: {
        play(choice) {
            this.aiChoice = this.choices[Math.floor(Math.random() * this.choices.length)];
            this.userChoice = choice;
            this.result = this.rules[this.userChoice][this.aiChoice];
            switch(this.result) {
                case 'win' : this.winCount++;break;
                case 'loss' : this.lossCount++;break;
                case 'draw' : this.drawCount++;break;
            }
            this.saveData();
            this.showResult = true;  
        },
        restart() {
            this.showResult = false;
            this.loadData();
        },
        saveData() {
          localStorage.setItem('win', this.winCount); 
          localStorage.setItem('draw', this.drawCount); 
          localStorage.setItem('loss', this.lossCount); 
        },
        loadData()
        {
            this.winCount = parseInt(localStorage.getItem('win')) || 0;
            this.drawCount = parseInt(localStorage.getItem('draw')) || 0;
            this.lossCount = parseInt(localStorage.getItem('loss')) || 0;
        },
    },
    mounted() {
        this.loadData();
    },
    computed : {
        calcualtePercentage() {
            this.total = this.winCount + this.lossCount + this.drawCount;
            return Math.floor((this.winCount/this.total)*100);
        }
    }
}
</script>

<style>

</style>
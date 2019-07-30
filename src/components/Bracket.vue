<template>
    <div class="wrapper has-text-centered">
        <button class="button is-info" @click='returnHome'>Return Home</button>
        <br>
        <h1 class="title is-size-6" v-if="mode == 'double' && !isFinal">Upper Bracket</h1>
        <div class="container1" v-if='!isFinal'>
            <div class="bracket-column" v-for='(item, index) in bracketLayout' :class="'row'+index">
                <div class="matchup" v-for='(n, index2) in item' v-if='n.round != 0 || submitted == true'>
                    <button class="button" 
                        :class="getColor(n, n.player1)"
                        @click='isWinner(n, n.player1, n.player2)'>
                        <p>{{ n.player1 }}</p>
                        <span class="icon is-small" v-if='n.winner == n.player1 && n.winner.length > 0'>
                            <i class="fas fa-check"></i>
                        </span>
                    </button>
                    <button class="button" 
                        :class="getColor(n, n.player2)"
                        @click='isWinner(n, n.player2, n.player1)'> 
                        <p>{{ n.player2 }}</p>
                        <span class="icon is-small" v-if='n.winner == n.player2 && n.winner.length > 0'>
                            <i class="fas fa-check"></i>
                        </span>
                    </button>
                </div>
                <div class="matchup" v-for='(n, index2) in item' v-if='n.round == 0 && submitted == false'>
                    <input class="input is-primary" v-model='n.player1' placeholder='Player Name' @click='isWinner(n, n.player1)'>
                    <input class="input is-primary" v-model='n.player2' placeholder='Player Name' @click='isWinner(n, n.player2)'>
                </div>
            </div>
        </div>
        <button class="button is-info submit-button" @click='lockPlayers' :disabled='submitted' v-if='!submitted'>Submit Players</button>
        <br v-if="mode == 'double' && submitted">
        <h1 class="title is-size-6" v-if="mode == 'double' && submitted && !isFinal">Lower Bracket</h1>
        <div class="container1" v-if="mode == 'double' && submitted && !isFinal">
            <div class="bracket-column" v-for='(item, index) in loserBracket' :class="'row'+index">
                <div class="matchup" v-for='(n, index2) in item'>
                    <button class="button" 
                        :class="getColor(n, n.player1)"
                        @click='isBottomWinner(n, n.player1)'>
                        <p>{{ n.player1 }}</p>
                        <span class="icon is-small" v-if='n.winner == n.player1 && n.winner.length > 0'>
                            <i class="fas fa-check"></i>
                        </span>
                    </button>
                    <button class="button" 
                        :class="getColor(n, n.player2)"
                        @click='isBottomWinner(n, n.player2)'> 
                        <p>{{ n.player2 }}</p>
                        <span class="icon is-small" v-if='n.winner == n.player2 && n.winner.length > 0'>
                            <i class="fas fa-check"></i>
                        </span>
                    </button>
                </div>
            </div>
        </div>
        <h1 class="title is-size-4" v-if='isFinal'>Grand Final</h1>
        <div class="container1" v-if='isFinal'>
            <div class="finalMatchup">
                <button class="button is-info" @click='congratulate(finalMatchup.player1)'> 
                    <p>{{finalMatchup.player1}}</p>
                </button>
                <p> VS. </p>
                <button class="button is-info" @click='congratulate(finalMatchup.player2)'> 
                    <p>{{finalMatchup.player2}}</p>
                </button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data: function() {
        return {
            bracketLayout: [],
            loserBracket: [],
            finalMatchup: {
                player1: '',
                player2: ''
            },
            submitted: false
        }
    },
    props: ['numPlayers', 'mode', 'players'],
    methods: {
        lockPlayers() {
            this.submitted = true;
            var inputs = document.querySelectorAll('.input').forEach((input) => {
                let button = document.createElement('button');
                button.className = 'button is-primary'
                input.parentNode.replaceChild(button, input);
            });
            console.log(this.bracketLayout);

        },
        isWinner(matchup, winner, loser) {

            matchup.winner = winner;
            matchup.loser = loser;
            var totalRounds = this.bracketLayout.length -1 ;

            var round;
            var lowRound;
            if (matchup.side == 'left') {
                round = matchup.round + 1;
                lowRound = matchup.round * 2 - 1;
                if (lowRound < 0) lowRound = 0;
            } else {
                round = totalRounds - matchup.round - 1;
                console.log('Round: ' + round);
            }
            
            if (round > totalRounds) {
                this.finalMatchup.player1 = winner;
                this.loserBracket[lowRound][0].player1 = loser;
                return true;
            } else if (this.mode == 'single' && matchup.side == 'left' && round > totalRounds / 2) {
                this.finalMatchup.player1 = winner;
                return true;
            } else if (this.mode == 'single' && matchup.side == 'right' && round < totalRounds / 2) {
                this.finalMatchup.player2 = winner;
                return true;
            }

            var nextMatchup;
            if (matchup.matchup % 2 == 0) {
                nextMatchup = matchup.matchup / 2;
                this.bracketLayout[round][nextMatchup].player1 = winner;
                this.loserBracket[lowRound][nextMatchup].player1 = loser;
            } else {
                nextMatchup = (matchup.matchup - 1) / 2;
                this.bracketLayout[round][nextMatchup].player2 = winner;
                if (lowRound == 0) {
                    this.loserBracket[lowRound][nextMatchup].player2 = loser;
                } else {
                    this.loserBracket[lowRound][nextMatchup+1].player1 = loser;
                }
            }
            console.log(winner);
        },
        isBottomWinner(matchup, winner) {
            matchup.winner = winner;
            var round;
            var nextMatchup;
            round = matchup.round + 1;
            if (round > this.loserBracket.length - 1) {
                this.finalMatchup.player2 = winner;
                return true;
            }

            if (round % 2 != 0) {
                this.loserBracket[round][matchup.matchup].player2 = winner;
            } else {
                if (matchup.matchup % 2 == 0) {
                    nextMatchup = matchup.matchup / 2;
                    this.loserBracket[round][nextMatchup].player1 = winner;
                } else {
                    nextMatchup = (matchup.matchup - 1) / 2;
                    this.loserBracket[round][nextMatchup].player2 = winner;
            }
            }
        },
        getColor(n, curPlayer) {
            if (n.winner.length > 0 && n.winner != curPlayer) {
                return 'is-danger broken';
            }
            else if ( n.winner.length > 0 && n.winner == curPlayer) {
                return 'is-success';
            }
            else if ( curPlayer ) {
                return 'is-info';
            }
            else {
                return 'is-dark';
            }
        },
        returnHome() {
            this.$emit('returnHome', true);
        },
        congratulate(finalWinner) {
            this.$emit('congrat', finalWinner);
        }
    },
    computed: {
        isFinal() {
            return this.finalMatchup.player1.length > 0 && this.finalMatchup.player2.length > 0;
        }
    },
    created() {

        var bracketDivider;
        if (this.mode == 'single') {
            bracketDivider = 4;
        }
        else if (this.mode == 'double') {
            bracketDivider = 2;
        }
        // Depth is equal to 
        // x = Math.ciel(Math.log(numPlayers) / Math.log(2)) + 1;
        var numMatchups = this.numPlayers / bracketDivider;
        var round = 0;
        while(numMatchups >= 1) {
            this.bracketLayout[round] = [];
            for (let matchup = 0; matchup < numMatchups; matchup++) {
                this.bracketLayout[round][matchup] = {
                    side: 'left',
                    round: round,
                    matchup: matchup,
                    player1: '',
                    player2: '',
                    winner: '',
                };
            }
            round++;
            numMatchups /= 2;
        }

        function copy(o) {
            var output, v, key;
            output = Array.isArray(o) ? [] : {};
            for (key in o) {
                v = o[key];
                output[key] = (typeof v === "object") ? copy(v) : v;
            }
            return output;
        }

        if (this.mode == 'single') {
            var copy1 = copy(this.bracketLayout);
            var copy2 = copy(this.bracketLayout);
            for (let i = 0; i < copy2.length; i++) {
                for (let j = 0; j < copy2[i].length; j++ ) {
                    //copy2[i][j].matchup += this.numPlayers / 4;
                    copy2[i][j].side = 'right';
                }
            }
            this.bracketLayout = copy1.concat(copy2.reverse());
        }
        else if (this.mode == 'double') {
            var loserMatchups = this.numPlayers / bracketDivider / 2;
            var loserRound = 0;
            while(loserMatchups >= 1) {
                this.loserBracket[loserRound] = [];
                this.loserBracket[loserRound + 1] = [];
                for (let matchup = 0; matchup < loserMatchups; matchup++) {
                    this.loserBracket[loserRound][matchup] = {
                        side: 'left',
                        round: loserRound,
                        matchup: matchup,
                        player1: '',
                        player2: '',
                        winner: '',
                        loser: ''
                    };
                    this.loserBracket[loserRound + 1][matchup] = {
                        side: 'left',
                        round: loserRound + 1,
                        matchup: matchup,
                        player1: '',
                        player2: '',
                        winner: '',
                        loser: ''
                    };
                }
                loserRound = loserRound + 2;
                loserMatchups /= 2;
            }
            var copy1 = copy(this.bracketLayout);
            this.bracketLayout = copy1;
            var copy2 = copy(this.loserBracket);
            this.loserBracket = copy2;
        }
        if (this.players.length > 0) {
            this.lockPlayers();
            let lastRound = this.bracketLayout.length - 1;
            for (var i = 0; i < this.bracketLayout[0].length; i++) {
                this.bracketLayout[0][i].player1 = this.players[i*2];
                this.bracketLayout[0][i].player2 = this.players[i*2+1];
            }  
            i=i+this.bracketLayout[0].length;
            for (let j = 0; j < this.bracketLayout[lastRound].length; j++ ) {
                this.bracketLayout[lastRound][j].player1 = this.players[i+j*2];
                this.bracketLayout[lastRound][j].player2 = this.players[i+j*2+1];
            }
        }
    }
}
</script>

<style>
.container1 {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
}
.wrapper {
    display: flex;
    flex-direction: column;
    position: absolute;
    top: 50%; left: 50%;
    transform: translate(-50%,-50%);
    width: auto;
    border-radius: 10px;
    background-color: white;
    overflow: hidden;
    box-shadow: rgba(0, 0, 0, 0.2) 0 30px 18px -24px;
    min-width: 850px;
}
.bracket-column {
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    align-items: center;
}
.matchup {
    padding: 10px;
    display: flex;
    flex-direction: column;
}
.finalMatchup {
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    width: 100%;
}
.matchup button, .matchup input {
    margin-top: 1px;
    margin-bottom: 1px;
    border: 1px solid green;
    width: 150px;
}
.finalMatchup button {
    width: 200px;
    height: 40px;
    margin: 20px 5px 50px 5px
}
.submit-button {
    margin-top: 20px;
}
.broken {
    opacity: 0.35;
}
.is-dark {
    opacity: 0.25;
}
</style>

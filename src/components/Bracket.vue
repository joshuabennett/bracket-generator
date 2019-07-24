<template>
    <div class="wrapper">
        <div class="container1">
            <div class="bracket-column" v-for='(item, index) in bracketLayout' :class="'row'+index">
                <div class="matchup" v-for='(n, index2) in item' v-if='n.round != 0 || submitted == true'>
                    <button class="button" 
                        :class="getColor(n, n.player1)"
                        @click='isWinner(n, n.player1)'>
                        {{ n.player1 }}
                    </button>
                    <button class="button" 
                        :class="getColor(n, n.player2)"
                        @click='isWinner(n, n.player2)'> 
                        {{ n.player2 }}
                    </button>
                </div>
                <div class="matchup" v-for='(n, index2) in item' v-if='n.round == 0 && submitted == false'>
                    <input class="input is-primary" v-model='n.player1' placeholder='Player Name' @click='isWinner(n, n.player1)'>
                    <input class="input is-primary" v-model='n.player2' placeholder='Player Name' @click='isWinner(n, n.player2)'>
                </div>
            </div>
        </div>
        <button class="button is-info submit-button" @click='lockPlayers' :disabled='submitted'>Submit Players</button>
    </div>
</template>

<script>
export default {
    data: function() {
        return {
            bracketLayout: [],
            submitted: false
        }
    },
    props: ['numPlayers', 'mode'],
    methods: {
        lockPlayers() {
            this.submitted = true;
            var inputs = document.querySelectorAll('.input').forEach((input) => {
                let button = document.createElement('button');
                button.className = 'button is-primary'
                button.innerHTML = input.getAttribute('placeholder');
                input.parentNode.replaceChild(button, input);
            });
            console.log(this.bracketLayout);

        },
        isWinner(matchup, winner) {

            matchup.winner = winner;
            var totalRounds = this.bracketLayout.length -1 ;

            var round;
            if (matchup.side == 'left') {
                round = matchup.round + 1;
            } else {
                round = totalRounds - matchup.round - 1;
                console.log('Round: ' + round);
            }
            var nextMatchup;
            if (matchup.matchup % 2 == 0) {
                nextMatchup = matchup.matchup / 2;
                this.bracketLayout[round][nextMatchup].player1 = winner;
            } else {
                nextMatchup = (matchup.matchup - 1) / 2;
                this.bracketLayout[round][nextMatchup].player2 = winner;
            }
            console.log(winner);
    
        },
        getColor(n, curPlayer) {
            if (n.winner.length > 0 && n.winner != curPlayer) {
                return 'is-danger broken';
            }
            else if ( n.winner.length > 0 && n.winner == curPlayer) {
                return 'is-primary';
            }
            else {
                return 'is-info';
            }
        }
    },
    created() {
        console.log(this.mode);
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
                }
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
            var copy1 = copy(this.bracketLayout);
            this.bracketLayout = copy1;
        }
    }
}
</script>

<style>
.container1 {
    display: flex;
    flex-direction: row;
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
.matchup button, .matchup input {
    margin-top: 1px;
    margin-bottom: 1px;
    border: 1px solid green;
    width: 150px;
}
.submit-button {
    margin-top: 20px;
}
.broken {
    opacity: 0.35;
}
</style>

<template>
    <div class="wrapper">
        <button class="button is-info" @click='returnHome'>Return Home</button>
        <div class="container1">
            <div class='panel'>
                <p class='panel-heading'>
                    Round Robin
                </p>
                <p class="panel-tabs">
                    <a v-for='n in Number(bracketInfo.groups)' 
                        @click='loadGroup(n)'
                        :class="{'is-active' : activeGroup == n - 1}">
                            Group {{n}} 
                    </a>
                </p>
                <div class="panel-block" v-for='n in curGroup' v-if='!submitted'>
                    <p class="control has-icons-left">
                        <input class="input is-small" type="text" placeholder="Player Name" v-model='n.name'>
                        <span class="icon is-small is-left">
                            <i class="fas fa-user"></i>
                        </span>
                    </p>
                </div>
                <a class="panel-block" v-for='(n, index) in curGroup' v-if='submitted' :class="{'top2' : (index < bracketInfo.cut) && n.wins > 0}">
                    <span class="panel-icon">
                        <i class="fas fa-user"></i>
                    </span>
                    <span class="tag is-info">
                        {{n.wins}} - {{n.losses}}
                    </span>
                    {{n.name}}
                </a>
            </div>
            <div class="matchup-container">
                <h1 class="title is-size-5 is-marginless">Matchups</h1>
                <div class="pairing" v-for='matchup in curMatchups' v-if='submitted'> 
                    <button class="button is-info" 
                        @click='addWin(matchup, matchup.player1, matchup.player2)'
                        :class="{'is-success': matchup.player1.name == matchup.winner}">
                        {{matchup.player1.name}}
                    </button>
                    <button class="button is-info" 
                        @click='addWin(matchup, matchup.player2, matchup.player1)'
                        :class="{'is-success': matchup.player2.name == matchup.winner}">
                        {{matchup.player2.name}}
                    </button>
                </div>
            </div>
        </div>
        <button class="button is-info submit-button" @click='submitPlayers' v-if='!submitted'>Submit Players</button>
        <button class="button is-success playoff-button" v-if='submitted' @click='playoffs'>Proceed to Playoffs</button>

    </div>
</template>

<script>
export default {
    data: function() {
        return {
            groups: [],
            matchups: [],
            activeGroup: 0,
            submitted: false
        }
    },
    props: ['bracketInfo'],
    methods: {
        loadGroup(num) {
            this.activeGroup = num - 1;
        },
        addWin(matchup, winner, loser) {
            matchup.winner = winner.name;
            winner.wins++;
            loser.losses++;
            this.groups.forEach( (group) => {
                group.sort( (a, b) => {
                    return (b.wins - a.wins)
                });
            });
            console.log(matchup);
        },
        submitPlayers() {
            for (let groupNum = 0; groupNum < this.groups.length; groupNum++) {
                this.matchups[groupNum] = [];
                for (let i = 0; i < this.groups[groupNum].length; i++) {
                    for (let j = i + 1; j < this.groups[groupNum].length; j++) {
                        this.matchups[groupNum].push({
                            player1: this.groups[groupNum][i],
                            player2: this.groups[groupNum][j],
                            winner: ''
                        });
                    }
                }
            }
            this.submitted = true;
            this.$forceUpdate();
        },
        returnHome() {
            this.$emit('returnHome', true);
        },
        playoffs() {
            var playoffBracket = {
                numPlayers: this.bracketInfo.cut * this.bracketInfo.groups,
                players: []
            }
            console.log(playoffBracket.numPlayers);
            var cutAmount = this.bracketInfo.cut;
            this.groups.forEach( (group) => {
                for (let i = 0; i < cutAmount; i++) {
                    playoffBracket.players.push(group[i].name);
                }
            });
            this.$emit('startPlayoff', playoffBracket);
        }
    },
    computed: {
        curGroup() {
            return this.groups[this.activeGroup];
        },
        curMatchups() {
            return this.matchups[this.activeGroup];
        }
    },
    created() {
        for (let groupNum = 0; groupNum < this.bracketInfo.groups; groupNum++) {
            this.$set(this.groups, groupNum, []);
            //this.groups[groupNum] = [];
            for (let playerNum = 0; playerNum < this.bracketInfo.numPlayers / this.bracketInfo.groups; playerNum++) {
             //   this.groups[groupNum][playerNum] = {
                let newPlayer = {
                    name: '',
                    wins: 0,
                    losses: 0,
                    tiebreaker: 0
                }
                this.$set(this.groups[groupNum], playerNum, newPlayer);
            }
        }
    }
}
</script>

<style>
.panel {
    min-width: 400px;
}
.wrapper {
    flex-direction: column;
}
.container1 {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    padding: 25px;
}
.matchup-container {
    display: flex;
    flex-direction: column;
}
.pairing button {
    min-width: 150px;
    margin: 2px;
}
.tag {
    margin-right: 10px;
}
.top2 {
    background: rgba(6, 245, 6, 0.432);
}
</style>

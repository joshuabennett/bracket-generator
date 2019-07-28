<template>
    <div class="wrapper">
        <div class="container1">
            <div class='panel'>
                <p class='panel-heading'>
                    Round Robin
                </p>
                <p class="panel-tabs">
                    <a v-for='n in Number(bracketInfo.groups)' @click='loadGroup(n)'>Group {{n}}</a>
                </p>
                <div class="panel-block" v-for='n in curGroup' v-if='!submitted'>
                    <p class="control has-icons-left">
                        <input class="input is-small" type="text" placeholder="Player Name" v-model='n.name'>
                        <span class="icon is-small is-left">
                            <i class="fas fa-user"></i>
                        </span>
                    </p>
                </div>
                <a class="panel-block" v-for='(n, index) in curGroup' v-if='submitted' :class="{'top2' : index == 0 || index == 1}">
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
                        @click='addWin(matchup, matchup.player2, matchup.player1)'>
                        {{matchup.player2.name}}
                    </button>
                </div>
            </div>
        </div>
        <button class="button is-info submit-button" @click='submitted = true'>Submit Players</button>
    </div>
</template>

<script>
export default {
    data: function() {
        return {
            groups: [],
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
        }
    },
    computed: {
        curGroup() {
            return this.groups[this.activeGroup];
        },
        curMatchups() {
            var matchups = [];
            for (let i = 0; i < this.curGroup.length; i++) {
                for (let j = i + 1; j < this.curGroup.length; j++) {
                    matchups.push({
                        player1: this.curGroup[i],
                        player2: this.curGroup[j],
                        winner: ''
                    });
                }
            }
        return matchups;
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
    min-width: 300px;
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

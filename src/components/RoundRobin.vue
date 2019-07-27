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
                <div class="panel-block" v-for='n in curGroup'>
                    <p class="control has-icons-left">
                        <input class="input is-small" type="text" placeholder="Player Name" v-model='n.name'>
                        <span class="icon is-small is-left">
                            <i class="fas fa-user"></i>
                        </span>
                    </p>
                </div>
            </div>
            <button class="button" v-for='n in curGroup'>{{n.name}} </button>
        </div>
        <button class="button is-info submit-button">Submit Players</button>
    </div>
</template>

<script>
export default {
    data: function() {
        return {
            groups: [],
            activeGroup: 0
        }
    },
    props: ['bracketInfo'],
    methods: {
        loadGroup(num) {
            this.activeGroup = num - 1;
        }
    },
    computed: {
        curGroup() {
            return this.groups[this.activeGroup];
        }
    },
    created() {
        for (let groupNum = 0; groupNum < this.bracketInfo.groups; groupNum++) {
            this.groups[groupNum] = [];
            for (let playerNum = 0; playerNum < this.bracketInfo.numPlayers / this.bracketInfo.groups; playerNum++) {
                this.groups[groupNum][playerNum] = {
                    name: '',
                    wins: 0,
                    losses: 0,
                    tiebreaker: 0
                }
            }
        }
    }
}
</script>

<style>
.panel {
    padding: 25px;
}
.wrapper {
    flex-direction: column;
}
.container1 {
    display: flex;
    flex-direction: row;
}
</style>

<template>
    <div class="container1">
        <div class="bracket-column" v-for='item in bracketLayout'>
            <div class="matchup" v-for='n in item'>
                <input class="input">
                <input class="input">
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data: function() {
        return {
            bracketLayout: []
        }
    },
    props: ['numPlayers'],
    created() {
        var counter = this.numPlayers / 2;
        while(counter > 1) {
            this.bracketLayout.push(counter / 2);
            counter = counter / 2;
        }
        this.bracketLayout.push(0);
        counter = 1;
        while(counter < this.numPlayers / 2) {
            this.bracketLayout.push(counter);
            counter = counter * 2;
        }
        console.log(this.bracketLayout);
    }
}
</script>

<style>
.container1 {
    display: flex;
    flex-direction: row;
    position: absolute;
    top: 50%; left: 50%;
    transform: translate(-50%,-50%);
    width: 1200px;
    border-radius: 10px;
    background-color: white;
    overflow: hidden;
    box-shadow: rgba(0, 0, 0, 0.2) 0 30px 18px -24px;
}
.bracket-column {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}
.matchup {
    padding: 10px;
}
.matchup input {
    margin-top: 1px;
    margin-bottom: 1px;
}
</style>

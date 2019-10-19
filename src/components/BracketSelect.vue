<template>
    <div class='wrapper'>
        <div class="poster">
            <span class="image">
                <img src='https://upload.wikimedia.org/wikipedia/commons/4/49/Smash_Ball.png'>
            </span>
        </div>
        <div class="bracket-data">
            <div class="field is-horizontal">
                <div class='field-label is-normal'>
                    <label class="label">Type of Bracket:</label>
                </div>
                <div class="field-body">
                    <div class="field">
                        <div class="control">
                            <div class="select">
                                <select v-model='bracket.mode'>
                                    <option disabled value=''>Select Bracket Type</option>
                                    <option value='single' >Single Elimination</option>
                                    <option value='double'>Double Elimination</option>
                                    <option value='robin'>Round Robin</option>
                                </select>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="field is-horizontal">
                <div class='field-label is-normal'>
                    <label class="label">Number of Players:</label>
                </div>
                <div class="field-body">
                    <div class="field">
                        <div class="control">
                            <input class="input is-primary" type="text" placeholder="4, 8, or 16" v-model='bracket.numPlayers' :class="{'is-danger' : isValidPlayers}">
                            <p class="help is-danger" v-if='isValidPlayers'>Please put a valid amount of players (4, 8, or 16) </p>
                        </div>
                    </div>
                </div>
            </div>
            <div class="field is-horizontal" v-if="bracket.mode == 'robin'">
                <div class='field-label is-normal'>
                    <label class="label">Number of Groups:</label>
                </div>
                <div class="field-body">
                    <div class="field">
                        <div class="control">
                            <input class="input is-primary" type="text" placeholder="Primary input" v-model='bracket.groups'
                            :class="{'is-danger' : isValidGroups}">
                            <p class="help is-danger" 
                            v-if='isValidGroups'>
                                Please put a valid number of groups. Cannot split {{bracket.groups}} groups evenly. 
                            </p>
                        </div>
                    </div>
                </div>
            </div>
                        <div class="field is-horizontal" v-if="bracket.mode == 'robin'">
                <div class='field-label is-normal'>
                    <label class="label">Cut to Top:</label>
                </div>
                <div class="field-body">
                    <div class="field">
                        <div class="control">
                            <input class="input is-primary" type="text" placeholder="Primary input" v-model='bracket.cut' :class="{'is-danger' : isValidCutoff}">
                            <p class="help is-danger" v-if='isValidCutoff'>Please put valid cutoff amount.</p>
                        </div>
                    </div>
                </div>
            </div>
            <div class="field is-horizontal">
                <div class="field-label">
                    <!-- Left empty for spacing -->
                </div>
                <div class="field-body">
                    <div class="field">
                        <div class="control">
                            <button class="button is-primary" @click='submitBracket' :disabled='isValidPlayers || isValidCutoff || isValidGroups'>
                            Submit
                            </button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data: function() {
        return {
            bracket: {
                mode: '',
                numPlayers: '',
                cut: 2,
                groups: 1
            }
        }
    },
    methods: {
        submitBracket() {
            this.$emit('updateBracket', this.bracket)
        }
    },
    computed: {
        isValidPlayers() {
            return this.bracket.numPlayers % 4 != 0 || this.bracket.numPlayers > 16;
        },
        isValidCutoff() {
            return this.bracket.numPlayers / this.bracket.groups <= this.bracket.cut || this.bracket.cut < 1;
        },
        isValidGroups() {
            return (this.bracket.numPlayers % this.bracket.groups) != 0 || Number(this.bracket.groups) >= Number(this.bracket.numPlayers);
        }
    }

}
</script>

<style>
html {
    background: whitesmoke;
}
.wrapper {
    display: flex;
    flex-direction: row;
    position: absolute;
    top: 50%; left: 50%;
    transform: translate(-50%,-50%);
    width: 850px;
    border-radius: 10px;
    background-color: white;
    overflow: hidden;
    box-shadow: rgba(0, 0, 0, 0.2) 0 30px 18px -24px;
}
.poster {
    padding-left: 30px;
    width: 200px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    position: relative;
    overflow: hidden;
}
.bracket-data {
    padding: 40px 30px 40px 10px;
    width: 100%;

}
.image {
    position: relative;
    overflow: hidden;
}
</style>

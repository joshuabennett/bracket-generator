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
                                <select v-model='selected'>
                                    <option disabled value=''>Select Bracket Type</option>
                                    <option >Single Elimination</option>
                                    <option >Double Elimination</option>
                                    <option >Round Robin</option>
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
                            <input class="input is-primary" type="text" placeholder="Primary input" v-model='bracket.numPlayers'>
                        </div>
                    </div>
                </div>
            </div>
            <div class="field is-horizontal" v-if="selected == 'Round Robin'">
                <div class='field-label is-normal'>
                    <label class="label">Number of Groups:</label>
                </div>
                <div class="field-body">
                    <div class="field">
                        <div class="control">
                            <input class="input is-primary" type="text" placeholder="Primary input" v-model='bracket.groups'>
                        </div>
                    </div>
                </div>
            </div>
                        <div class="field is-horizontal" v-if="selected == 'Round Robin'">
                <div class='field-label is-normal'>
                    <label class="label">Cut to Top:</label>
                </div>
                <div class="field-body">
                    <div class="field">
                        <div class="control">
                            <input class="input is-primary" type="text" placeholder="Primary input" v-model='bracket.cut'>
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
                            <button class="button is-primary" @click='submitBracket'>
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
            selected: '',
            bracket: {
                numPlayers: 0,
                cut: 2,
                groups: 1
            }
        }
    },
    methods: {
        submitBracket() {
            this.$emit('updateBracket', this.bracket)
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
img {
}
</style>

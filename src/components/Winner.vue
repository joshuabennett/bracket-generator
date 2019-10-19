<template>
    <div class="flex-wrapper flex-wrap">
        <div class="bracket-wrapper win-wrap">
            <div class="container4 has-text-centered">
                <button class="button is-info" @click='returnHome'>Start New Bracket</button>
                <h2 class="title is-size-4 is-marginless">Congratulations!</h2>
                <h2 class="title is-size-1">{{ finalWinner }}</h2>          
            </div>
        </div>
        <canvas id="canvas1"></canvas>
    </div>
</template>

<script>
import ConfettiGenerator from "confetti-js";

export default {
    props: ['finalWinner'],
    methods: {
        returnHome() {
            this.$emit('returnHome', true);
        }
    },
    mounted() {
        var canvas1Settings = {
            target: 'canvas1',
            max: 200,
            props: ['square', 'triangle', 'line'],
            colors: [[165,104,246],[230,61,135],[0,199,228],[253,214,126]],
            clock: 5
        };
        var canvas1 = new ConfettiGenerator(canvas1Settings);
        canvas1.render();
    }
}
</script>

<style>
    .win-wrap {
        position: absolute;
            top: 50%; left: 50%;
    transform: translate(-50%,-50%);
    }
    .flex-wrap {
        position: relative;
    }
    .container4 {
        display: flex;
        flex-direction: column;
        z-index: 9;
    }
    .container4 h2 {
        padding: 25px;
    }
    .canvas {
        z-index: 1;
        position:absolute;
    }
</style>

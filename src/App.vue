<template>
  <div>
    <bracket-select v-if='!submitted' @updateBracket='updateInfo'></bracket-select>
    <bracket v-if="submitted && (bracket.mode == 'single' || bracket.mode == 'double')" :numPlayers='bracket.numPlayers' :mode='bracket.mode' :players='bracket.players' @returnHome='newBracket' @congrat='createWinner'></bracket>
    <round-robin v-if="bracket.mode == 'robin'" :bracketInfo='bracket'  @returnHome='newBracket' @startPlayoff='startPlayoff'></round-robin>
    <winner v-if="bracket.mode == 'final'" :finalWinner='finalWinner' @returnHome='newBracket'></winner>
  </div>
</template>

<script>
import BracketSelect from './components/BracketSelect.vue';
import Bracket from './components/Bracket.vue';
import RoundRobin from './components/RoundRobin.vue';
import Winner from './components/Winner.vue';

export default {
  data: function() {
    return {
      submitted: false,
      bracket: {},
      finalWinner: ''
    }
  },
  components: {
    'bracket-select': BracketSelect,
    'bracket': Bracket,
    'round-robin': RoundRobin,
    'winner' : Winner
  },
  methods: {
    updateInfo(e) {
      this.bracket = e;
      this.submitted = true;
    },
    newBracket(e) {
      this.bracket = {},
      this.submitted = false;
      this.finalWinner = '';
    },
    startPlayoff(e) {
      this.bracket.mode = 'single';
      this.bracket.numPlayers = e.numPlayers;
      this.bracket.players = e.players;
    },
    createWinner(e) {
      this.bracket.mode='final';
      this.finalWinner = e;

    }
  }
}
</script>

<style>
.body {
  background-color: whitesmoke;
}
</style>


<template>
  <div>
    <bracket-select v-if='!submitted' @updateBracket='updateInfo'></bracket-select>
    <bracket v-if="submitted && bracket.mode != 'robin'" :numPlayers='bracket.numPlayers' :mode='bracket.mode' :players='bracket.players' @returnHome='newBracket'></bracket>
    <round-robin v-if="bracket.mode == 'robin'" :bracketInfo='bracket'  @returnHome='newBracket' @startPlayoff='startPlayoff'></round-robin>
  </div>
</template>

<script>
import BracketSelect from './components/BracketSelect.vue';
import Bracket from './components/Bracket.vue';
import RoundRobin from './components/RoundRobin.vue';

export default {
  data: function() {
    return {
      submitted: false,
      bracket: {},
    }
  },
  components: {
    'bracket-select': BracketSelect,
    'bracket': Bracket,
    'round-robin': RoundRobin
  },
  methods: {
    updateInfo(e) {
      this.bracket = e;
      this.submitted = true;
    },
    newBracket(e) {
      this.bracket = {},
      this.submitted = false;
    },
    startPlayoff(e) {
      this.bracket.mode = 'single';
      this.bracket.numPlayers = e.numPlayers;
      this.bracket.players = e.players;
    }
  }
}
</script>

<style>
.body {
  background-color: whitesmoke;
}
</style>


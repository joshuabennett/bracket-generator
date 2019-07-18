<template>
  <div>
    <bracket-select v-if='!submitted' @updateBracket='updateInfo'></bracket-select>
    <bracket v-if='submitted' :numPlayers='bracket.numPlayers'></bracket>
  </div>
</template>

<script>
import BracketSelect from './components/BracketSelect.vue';
import Bracket from './components/Bracket.vue';

export default {
  data: function() {
    return {
      submitted: false,
      bracket: {}
    }
  },
  components: {
    'bracket-select': BracketSelect,
    'bracket': Bracket
  },
  methods: {
    updateInfo(e) {
      this.bracket = e;
      this.submitted = true;
    }
  }
}
</script>

<style>
.body {
  background-color: whitesmoke;
}
</style>


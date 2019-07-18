<template>
  <div>
    <bracket-select></bracket-select>
  </div>
</template>

<script>
import BracketSelect from './components/BracketSelect.vue';

export default {
  components: {
    'bracket-select': BracketSelect
  }
}
</script>

<style>
.body {
  background-color: whitesmoke;
}
</style>


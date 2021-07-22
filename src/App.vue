<template>
  <Puzzles />
  <Records />
  <slider-puzzle :puzzleId="selectedPuzzleId" />
</template>

<script>
import Puzzles from "./components/Puzzles.vue";
import Records from "./components/Records.vue";
import SliderPuzzle from "./components/SliderPuzzle.vue";

export default {
  name: "app",
  components: { Puzzles, Records, SliderPuzzle },
  data() {
    return {
      selectedPuzzleId: "cut-red",
    };
  },
};
</script>
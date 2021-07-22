<template>
  <div>
    <h1>Swap the Images to Win</h1>
    <button @click="start" id="start-button">Start Game</button>

    <button @click="stop" id="quit-button">Quit</button>
    <p>Elapsed Time: {{ elapsedTime }}</p>
    <p v-if="isWinning">You win</p>
    <div class="row">
      <div
        class="column"
        v-for="(s, index) of shuffledPuzzleArray"
        v-bind:key="s"
        @click="swap(index)"
        v-bind:style="
          indexesToSwap.includes(index)
            ? 'border-style:solid'
            : 'border-style:none'
        "
      >
        <img v-bind:src="require(`../assets/${puzzleId}/${s}`)" />
      </div>
    </div>
  </div>
</template>

<script>
import moment from "moment";

const correctPuzzleArray = [
  "image_part_001.jpg",
  "image_part_002.jpg",
  "image_part_003.jpg",
  "image_part_004.jpg",
  "image_part_005.jpg",
  "image_part_006.jpg",
  "image_part_007.jpg",
  "image_part_008.jpg",
  "image_part_009.jpg",
];
export default {
  name: "SliderPuzzle",
  props: {
    puzzleId: {
      type: String,
      default: "cut-pink",
    },
  },

  data() {
    return {
      correctPuzzleArray,
      shuffledPuzzleArray: [...correctPuzzleArray].sort(
        () => Math.random() - 0.5
      ),
      startDateTime: new Date(),
      currentDateTime: new Date(),
      timer: undefined,
      indexesToSwap: [],
      isSelected: false,
    };
  },
  methods: {
    swap(index) {
      if (!this.timer) {
        return;
      }
      console.log(index, "selected");
      if (this.indexesToSwap.length < 2) {
        this.indexesToSwap.push(index);
      }
      if (this.indexesToSwap.length === 2) {
        const [index1, index2] = this.indexesToSwap;
        [this.shuffledPuzzleArray[index1], this.shuffledPuzzleArray[index2]] = [
          this.shuffledPuzzleArray[index2],
          this.shuffledPuzzleArray[index1],
        ];
        this.indexesToSwap = [];
      }
    },
    start() {
      console.log("Start");
      this.resetTime();
      this.shuffledPuzzleArray = [...correctPuzzleArray].sort(
        () => Math.random() - 0.5
      );
      this.indexesToSwap = [];
      this.timer = setInterval(() => {
        this.currentDateTime = new Date();
        if (this.isWinning) {
          this.recordSpeedRecords();
          this.stop();
        }
      }, 1000);
    },
    stop() {
      this.resetTime();
      clearInterval(this.timer);
      //  this.timer = undefined;
    },
    resetTime() {
      this.startDateTime = new Date();
      this.currentDateTime = new Date();
    },
    recordSpeedRecords() {
      let records = JSON.parse(localStorage.getItem("records")) || [];
      const { elapsedTime, elapsedDiff } = this;
      records.push({ elapsedTime, elapsedDiff });
      const sortedRecords = records
        .sort((a, b) => a.elapsedDiff - b.elapsedDiff)
        .slice(0, 10);
      const stringifiedRecords = JSON.stringify(sortedRecords);
      localStorage.setItem("records", stringifiedRecords);
    },
  },
  computed: {
    isWinning() {
      for (let i = 0; i < correctPuzzleArray.length; i++) {
        if (correctPuzzleArray[i] !== this.shuffledPuzzleArray[i]) {
          return false;
        }
      }
      return true;
    },
    elapsedDiff() {
      const currentDateTime = moment(this.currentDateTime);
      const startDateTime = moment(this.startDateTime);
      return currentDateTime.diff(startDateTime);
    },
    elapsedTime() {
      return moment.utc(this.elapsedDiff).format("HH:mm:ss");
    },
  },
};
</script>

<style scoped>
.row {
  display: flex;
  max-width: 90vw;
  flex-wrap: wrap;
}
.column {
  flex-grow: 5;
  width: 33%;
}
.column img {
  max-width: 100%;
}
.column .img_selected {
  border: solid;
}
</style>
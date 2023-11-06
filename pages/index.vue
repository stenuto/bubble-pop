<template>
  <div
    :style="{ backgroundImage: 'url(/bg1.png)' }"
    class="relative h-screen w-screen bg-no-repeat bg-center bg-cover overflow-hidden"
  >
    <Link
      rel="apple-touch-icon"
      sizes="180x180"
      href="./apple-touch-icon.png"
    />
    <Meta
      name="viewport"
      content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no"
    />
    <Bubble v-for="n in bubbleArray" :key="n" @popped="() => score++" />
    <div class="absolute top-0 left-0 right-0 p-4 flex gap-4">
      <div class="flex-1">
        <button
          @click="restart"
          class="bg-amber-500 hover:bg-amber-600 text-white font-bold py-2 px-4 rounded"
        >
          Restart
        </button>
      </div>
      <div class="flex items-center space-x-2">
        <div class="text-2xl font-bold text-white">Score:</div>
        <div class="text-2xl font-bold text-white">{{ score }}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      score: 0,
      counter: 0,
      intervalTime: 1000, // Start with a 1-second interval
      gameOver: false,
    };
  },
  mounted() {
    this.startBubbleGeneration();
  },
  computed: {
    bubbleArray() {
      return Array.from({ length: this.counter }, (_, i) => i);
    },
  },
  watch: {
    counter() {
      console.log("counter changed", this.counter);
    },
  },
  methods: {
    startBubbleGeneration() {
      const intervalId = setInterval(() => {
        if (this.gameOver) {
          clearInterval(intervalId); // Stop the loop if the game is over
        } else {
          this.counter += 1; // Increment the counter

          // Every 10 bubbles, decrease the interval time by 0.1 seconds
          if (this.counter % 10 === 0) {
            clearInterval(intervalId); // Clear the current interval
            this.intervalTime = Math.max(100, this.intervalTime - 100); // Decrease time but not less than 100ms
            this.startBubbleGeneration(); // Start a new interval with the updated time
          }
        }
      }, this.intervalTime);
    },
    restart() {
      this.score = 0;
      this.counter = 0;
      this.intervalTime = 1000;
      this.gameOver = false;
      this.startBubbleGeneration();
    },
  },
};
</script>

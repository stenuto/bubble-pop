<template>
  <div
    @click="pop"
    :style="{
      backgroundImage: 'url(/bubble.png)',
      transitionDuration: '5s',
      left: randomPosition() + '%',
      width: width * 200 + 'px',
      height: width * 200 + 'px',
    }"
    :class="[
      bubbleFloating ? '-translate-y-72' : 'translate-y-[100vh]',
      bubbleVisible ? 'block' : 'hidden',
      'bubble absolute z-100 bg-no-repeat bg-center bg-cover transition-transform ease-linear translate-x-1/2 rounded-full active:bg-white/30',
    ]"
  >
    <!-- Bubble content -->
  </div>
</template>

<script>
export default {
  data() {
    return {
      bubbleFloating: false,
      bubbleVisible: true,
      width: null,
    };
  },
  methods: {
    randomPosition() {
      return Math.floor(Math.random() * (80 - 20 + 1)) + 10;
    },
    randomSize() {
      // random number between .5 and 1
      this.width = Math.random() * (1 - 0.5) + 0.5;
    },
    pop() {
      this.bubbleVisible = false;
      this.playPopSound();
      this.$emit("popped");
    },
    playPopSound() {
      const audio = new Audio("./pop.mp3");
      audio
        .play()
        .catch((error) => console.error("Error playing the sound:", error));
    },
  },
  mounted() {
    this.randomSize();
    setTimeout(() => {
      this.bubbleFloating = true;
    }, 100);
  },
};
</script>

<style></style>

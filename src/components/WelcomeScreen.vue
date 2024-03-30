<template>
  <div class="welcome-screen" v-if="show">
    <canvas id="confetti-canvas"></canvas>
    <h1>Bienvenue au mariage de Romain et Amandine</h1>
  </div>
</template>

<script>
import confetti from 'canvas-confetti';

export default {
  data() {
    return {
      show: true
    };
  },
  mounted() {
    this.launchConfetti();
    setTimeout(() => {
      this.show = false;
      this.$emit('welcomeEnd');
    }, 3000);
  },
  methods: {
    launchConfetti() {
      const canvas = document.getElementById('confetti-canvas');
      const myConfetti = confetti.create(canvas, { resize: true });
      myConfetti({
        particleCount: 100,
        spread: 160
        // Vous pouvez ajouter d'autres options ici selon vos besoins
      });
    }
  }
};
</script>

<style>
.welcome-screen {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: white;
  z-index: 1000;
}

.welcome-screen h1 {
  position: absolute;
  font-size: 2em;
  text-align: center;
}

#confetti-canvas {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
</style>

<template>
  <main
    class="w-full min-h-screen bg-love-dark text-white overflow-hidden relative"
  >
    <!-- Background Music Player (Hidden) -->
    <audio ref="bgMusic" loop>
      <source src="/assets/bg-music.mp3" type="audio/mp3" />
    </audio>

    <!-- Tombol Mute/Unmute di Pojok Kanan Atas -->
    <button
      @click="toggleMusic"
      class="fixed top-4 right-4 z-50 p-3 rounded-full bg-white/10 backdrop-blur-md border border-white/20 hover:bg-white/20 transition-all"
    >
      {{ isPlaying ? "🔊" : "🔇" }}
    </button>

    <!-- TRANSISI ANTAR STAGE -->
    <Transition name="fade" mode="out-in">
      <!-- STAGE 1: LOGIN -->
      <Stage1_TheGate v-if="currentStage === 1" @unlock="nextStage" />

      <!-- STAGE 2: KUIS -->
      <Stage2_MemoryLane v-else-if="currentStage === 2" @complete="nextStage" />

      <!-- STAGE 3: HADIAH -->
      <Stage3_Reward v-else-if="currentStage === 3" />
    </Transition>
  </main>
</template>

<script setup lang="ts">
import { ref } from "vue";
// Import Components
import Stage1_TheGate from "./Components/Stage1_TheGate.vue";
import Stage2_MemoryLane from "./Components/Stage2_MemoryLane.vue";
import Stage3_Reward from "./Components/Stage3_Reward.vue";

const currentStage = ref(1);
const isPlaying = ref(false);
const bgMusic = ref<HTMLAudioElement | null>(null);

// Fungsi Pindah Stage
const nextStage = () => {
  currentStage.value++;
  // Otomatis play music saat masuk Stage 2 kalau belum nyala
  if (currentStage.value === 2 && !isPlaying.value) {
    toggleMusic();
  }
};

// Fungsi Musik
const toggleMusic = () => {
  if (bgMusic.value) {
    if (isPlaying.value) {
      bgMusic.value.pause();
    } else {
      bgMusic.value
        .play()
        .catch((e) => console.log("Perlu interaksi user dulu", e));
    }
    isPlaying.value = !isPlaying.value;
  }
};
</script>

<style>
/* Animasi Transisi Halus antar Stage */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.8s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>

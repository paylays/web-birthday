<template>
  <div
    class="min-h-screen relative flex flex-col items-center justify-center bg-[#0f0518] overflow-hidden p-6 perspective-1000"
  >
    <!-- BACKGROUND: Fireworks/Stars -->
    <div class="absolute inset-0 overflow-hidden">
      <div
        v-for="n in 30"
        :key="n"
        class="firework-particle"
        :style="getParticleStyle()"
      ></div>
    </div>

    <!-- MAIN CONTENT: Hadiah Box -->
    <Transition name="scale-up" appear>
      <div
        v-if="!isOpen"
        class="z-20 text-center cursor-pointer group"
        @click="openGift"
      >
        <div class="relative inline-block animate-bounce-slow">
          <div
            class="text-[100px] filter drop-shadow-[0_0_30px_rgba(236,72,153,0.6)] group-hover:scale-110 transition-transform duration-300"
          >
            🎁
          </div>
          <div
            class="absolute -bottom-4 left-1/2 -translate-x-1/2 w-20 h-4 bg-black/50 blur-lg rounded-full"
          ></div>
        </div>
        <h2
          class="mt-8 text-2xl text-white font-serif tracking-widest opacity-80 group-hover:opacity-100 transition-opacity"
        >
          Tap to Open
        </h2>
      </div>
    </Transition>

    <!-- THE LETTER (Modal) -->
    <Transition name="letter-open">
      <div
        v-if="isOpen"
        class="relative z-30 bg-[#fff9f0] w-full max-w-sm md:max-w-md p-8 md:p-12 rounded-lg shadow-2xl transform rotate-1"
      >
        <!-- Paper Texture & Fold Lines -->
        <div
          class="absolute inset-0 border-t-8 border-b-8 border-black/5 pointer-events-none"
        ></div>
        <div
          class="absolute top-0 left-0 w-full h-full bg-[url('https://www.transparenttextures.com/patterns/paper.png')] opacity-30"
        ></div>

        <!-- Content -->
        <div class="relative text-center">
          <div
            class="inline-block border-2 border-red-800 text-red-800 px-4 py-1 text-xs font-bold tracking-[0.2em] mb-6 rotate-[-2deg]"
          >
            BIRTHDAY VOUCHER
          </div>

          <h1 class="font-serif text-4xl text-gray-900 mb-6 leading-tight">
            Special Gift<br />
            <span class="text-pink-600 italic text-3xl">For You</span>
          </h1>

          <p class="font-serif text-gray-600 mb-8 leading-relaxed">
            Karena cindy udah jadi pacar yang hebat dan berhasil lewatin semua
            tantangan tadi, ini hadiah kecil buat cindy:
          </p>

          <!-- COUPON AREA -->
          <div
            class="bg-red-50 border-2 border-dashed border-red-300 p-6 rounded-xl relative group hover:bg-red-100 transition-colors cursor-default"
          >
            <div
              class="absolute -left-3 top-1/2 -translate-y-1/2 w-6 h-6 bg-[#fff9f0] rounded-full border-r-2 border-red-300"
            ></div>
            <div
              class="absolute -right-3 top-1/2 -translate-y-1/2 w-6 h-6 bg-[#fff9f0] rounded-full border-l-2 border-red-300"
            ></div>

            <h3 class="text-2xl font-bold text-red-600 mb-1">
              FREE MAKAN MAKAN
            </h3>
            <p class="text-sm text-red-400 uppercase tracking-widest">
              Anywhere You Want
            </p>
          </div>

          <p class="mt-8 text-xs text-gray-400 font-mono">
            *Screenshoot ini dan kirim ke iam untuk klaim yeee!
          </p>

          <!-- Close / Reset -->
          <button
            @click="reloadPage"
            class="mt-8 text-gray-400 hover:text-red-500 text-sm transition-colors flex items-center justify-center gap-2 mx-auto"
          >
            ↺ Ulangi Dari Awal
          </button>
        </div>
      </div>
    </Transition>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";

const isOpen = ref(false);

const openGift = () => {
  isOpen.value = true;
  // Trigger Confetti Logic Here if needed
};

const reloadPage = () => {
  window.location.reload();
};

const getParticleStyle = () => {
  const top = Math.random() * 100 + "%";
  const left = Math.random() * 100 + "%";
  const delay = Math.random() * 5 + "s";
  return {
    top,
    left,
    animationDelay: delay,
  };
};
</script>

<style scoped>
.perspective-1000 {
  perspective: 1000px;
}

/* Letter Animation */
.letter-open-enter-active {
  animation: letterIn 1s cubic-bezier(0.2, 0.8, 0.2, 1);
}
@keyframes letterIn {
  0% {
    transform: translateY(100px) scale(0.5) rotateX(90deg);
    opacity: 0;
  }
  100% {
    transform: translateY(0) scale(1) rotateX(0);
    opacity: 1;
  }
}

/* Fireworks Particles (CSS Only) */
.firework-particle {
  position: absolute;
  width: 6px;
  height: 6px;
  background: white;
  border-radius: 50%;
  box-shadow:
    0 0 10px #fff,
    0 0 20px #ff00ff,
    0 0 30px #ff00ff;
  opacity: 0;
  animation: firework 3s infinite;
}

@keyframes firework {
  0% {
    transform: scale(0);
    opacity: 1;
  }
  50% {
    opacity: 1;
  }
  100% {
    transform: scale(3);
    opacity: 0;
  }
}

.animate-bounce-slow {
  animation: bounce 3s infinite;
}
</style>

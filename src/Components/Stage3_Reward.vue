<template>
  <div class="min-h-screen relative flex flex-col items-center justify-center bg-gradient-to-t from-[#2d1b4e] to-[#1a0b2e] overflow-hidden p-4">
    
    <!-- CSS Confetti Container -->
    <div class="confetti-container absolute inset-0 pointer-events-none overflow-hidden">
      <div v-for="n in 50" :key="n" class="confetti" :style="getConfettiStyle(n)"></div>
    </div>

    <!-- Main Content -->
    <div v-if="!showLetter" class="text-center z-10 animate-scale-up">
      <h1 class="text-5xl md:text-6xl font-serif text-transparent bg-clip-text bg-gradient-to-r from-pink-300 via-purple-300 to-indigo-300 mb-6 drop-shadow-[0_0_15px_rgba(236,72,153,0.6)]">
        Happy Birthday!
      </h1>
      <p class="text-lg text-purple-200 mb-8 max-w-md mx-auto">
        Selamat sayang! Kamu berhasil menyelesaikan semua misi. <br>
        Aku punya sesuatu spesial buat kamu.
      </p>

      <button
        @click="openGift"
        class="group relative inline-flex items-center justify-center px-8 py-4 font-bold text-white transition-all duration-200 bg-purple-600 font-lg rounded-full focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-purple-600 hover:bg-purple-700 hover:shadow-[0_0_20px_rgba(168,85,247,0.7)] hover:scale-105"
      >
        <span class="absolute inset-0 w-full h-full -mt-1 rounded-full opacity-30 bg-gradient-to-b from-transparent via-transparent to-black"></span>
        <span class="relative mr-2 text-2xl">🎁</span>
        <span class="relative">Klaim Hadiah</span>
      </button>
    </div>

    <!-- The Letter / Voucher Modal -->
    <Transition name="pop">
      <div v-if="showLetter" class="z-50 w-full max-w-md bg-[#fff9f0] text-gray-800 p-8 rounded-sm shadow-2xl relative rotate-1 transform transition-all duration-500 hover:rotate-0">
        <!-- Paper Texture Effect -->
        <div class="absolute inset-0 opacity-10 bg-[url('https://www.transparenttextures.com/patterns/paper.png')] pointer-events-none"></div>
        
        <!-- Stamp / Decoration -->
        <div class="absolute -top-6 -right-6 w-20 h-20 bg-red-800 rounded-full flex items-center justify-center text-white text-xs font-bold rotate-12 shadow-lg border-4 border-white/50">
          OFFICIAL<br>VOUCHER
        </div>

        <h2 class="text-3xl font-serif text-red-700 mb-6 text-center border-b-2 border-red-200 pb-4">
          Tiket Makan Gratis
        </h2>
        
        <div class="space-y-4 font-serif text-lg leading-relaxed">
          <p>
            Dear <span class="font-bold text-purple-700">Sayangku</span>,
          </p>
          <p>
            Karena kamu udah pinter banget jawab kuis tadi, ini hadiah buat kamu:
          </p>
          <div class="bg-red-50 border-2 border-dashed border-red-300 p-4 rounded-lg text-center font-bold text-xl text-red-600 my-4">
            MAKAN SEPUASNYA <br>
            <span class="text-sm font-normal text-gray-600">(Di tempat favorit kamu!)</span>
          </div>
          <p class="text-sm text-gray-500 italic text-center">
            *Berlaku kapan aja, tinggal bilang aku. <br>
            Love you! ❤️
          </p>
        </div>

        <button 
          @click="showLetter = false"
          class="absolute -bottom-12 left-1/2 transform -translate-x-1/2 text-white/70 text-sm hover:text-white transition-colors"
        >
          (Tutup Surat)
        </button>
      </div>
    </Transition>

    <!-- Fireworks Overlay (Simple CSS implementation or logic placeholder) -->
    <div v-if="showFireworks" class="absolute inset-0 pointer-events-none">
       <!-- Fireworks implemented via generic confetti for now to save complexity -->
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';

const showLetter = ref(false);
const showFireworks = ref(false);

const openGift = () => {
  showLetter.value = true;
  showFireworks.value = true;
};

// Simple Random Generator for CSS Confetti
const getConfettiStyle = (n: number) => {
  const left = Math.random() * 100 + '%';
  const delay = Math.random() * 5 + 's';
  const duration = (Math.random() * 3 + 2) + 's';
  const bgColors = ['#ff0000', '#00ff00', '#0000ff', '#ffff00', '#ff00ff', '#00ffff'];
  const color = bgColors[Math.floor(Math.random() * bgColors.length)];
  
  return {
    left: left,
    animationDelay: delay,
    animationDuration: duration,
    backgroundColor: color,
  };
};

onMounted(() => {
  // Trigger initial celebration
});
</script>

<style scoped>
.animate-scale-up {
  animation: scaleUp 0.8s cubic-bezier(0.175, 0.885, 0.32, 1.275) forwards;
}

@keyframes scaleUp {
  from { opacity: 0; transform: scale(0.8); }
  to { opacity: 1; transform: scale(1); }
}

/* CSS Confetti Animation */
.confetti {
  position: absolute;
  top: -10px;
  width: 10px;
  height: 10px;
  opacity: 0.7;
  animation-name: fall;
  animation-timing-function: linear;
  animation-iteration-count: infinite;
}

@keyframes fall {
  0% { transform: translateY(-10vh) rotate(0deg); }
  100% { transform: translateY(110vh) rotate(720deg); }
}

/* Modal Pop Animation */
.pop-enter-active {
  animation: popIn 0.5s cubic-bezier(0.68, -0.55, 0.265, 1.55);
}
.pop-leave-active {
  transition: opacity 0.3s ease;
}
.pop-leave-to {
  opacity: 0;
}

@keyframes popIn {
  0% { transform: scale(0) rotate(-10deg); opacity: 0; }
  100% { transform: scale(1) rotate(1deg); opacity: 1; }
}
</style>
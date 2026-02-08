<template>
  <div
    class="stage-container relative w-full h-screen overflow-hidden bg-gradient-to-br from-[#1a0b2e] via-[#2d1b4e] to-[#0f0518] flex flex-col items-center justify-center text-white"
  >
    <!-- BACKGROUND: Animated Floating Particles (CSS) -->
    <div class="absolute inset-0 z-0 overflow-hidden">
      <div v-for="n in 20" :key="n" class="floating-orb" :style="getOrbStyle(n)"></div>
    </div>
    
    <!-- BACKGROUND: Interactive Mouse Paralax Effect -->
    <div class="absolute inset-0 z-0 bg-[url('https://www.transparenttextures.com/patterns/stardust.png')] opacity-30 animate-pulse-slow"></div>

    <!-- CONTENT: Login Form -->
    <Transition name="fade-slide" mode="out-in">
      <div
        v-if="!isUnlocked"
        class="z-10 w-4/5 max-w-sm backdrop-blur-xl bg-white/10 border border-white/20 p-8 rounded-3xl shadow-[0_0_50px_rgba(139,92,246,0.4)] relative group"
      >
        <!-- Decorative Glow -->
        <div class="absolute -inset-1 bg-gradient-to-r from-pink-500 to-purple-600 rounded-3xl blur opacity-25 group-hover:opacity-50 transition duration-1000 group-hover:duration-200"></div>

        <div class="relative">
          <h1
            class="text-4xl font-serif text-center mb-2 text-transparent bg-clip-text bg-gradient-to-r from-purple-200 to-pink-200 drop-shadow-sm"
          >
            Welcome Home
          </h1>
          <p
            class="text-xs text-center text-purple-200/80 mb-8 font-medium tracking-[0.2em] uppercase"
          >
            A Special Place For Us
          </p>

          <!-- QUESTION 1 -->
          <div v-if="step === 1" class="space-y-5">
            <label class="block text-sm font-medium text-purple-300 text-center"
              >Kapan kita jadian?</label
            >
            <input
              v-model="inputDate"
              type="text"
              placeholder="DDMMYYYY"
              class="w-full bg-black/40 border border-purple-500/30 rounded-xl px-4 py-4 text-center text-xl tracking-[0.3em] text-white placeholder-gray-500 focus:outline-none focus:border-purple-400 focus:ring-2 focus:ring-purple-500/50 transition-all duration-300"
              @keyup.enter="validateDate"
            />
            <p
              v-if="errorMsg"
              class="text-red-400 text-xs text-center font-bold animate-bounce"
            >
              {{ errorMsg }}
            </p>
            <button
              @click="validateDate"
              class="w-full bg-gradient-to-r from-purple-600 to-pink-600 hover:from-purple-500 hover:to-pink-500 text-white py-3 rounded-xl font-bold shadow-lg shadow-purple-900/50 transform transition-all active:scale-95 hover:-translate-y-1"
            >
              Jawab ❤️
            </button>
          </div>

          <!-- QUESTION 2 -->
          <div v-else-if="step === 2" class="space-y-5">
            <label class="block text-sm font-medium text-purple-300 text-center"
              >Panggilan sayang aku ke kamu?</label
            >
            <input
              v-model="inputName"
              type="text"
              placeholder="???"
              class="w-full bg-black/40 border border-purple-500/30 rounded-xl px-4 py-4 text-center text-xl font-bold tracking-widest text-white placeholder-gray-500 focus:outline-none focus:border-pink-400 focus:ring-2 focus:ring-pink-500/50 transition-all duration-300 uppercase"
              @keyup.enter="validateName"
            />
            <p
              v-if="errorMsg"
              class="text-red-400 text-xs text-center font-bold animate-bounce"
            >
              {{ errorMsg }}
            </p>
            <button
              @click="validateName"
              class="w-full bg-gradient-to-r from-pink-600 to-purple-600 hover:from-pink-500 hover:to-purple-500 text-white py-3 rounded-xl font-bold shadow-lg shadow-pink-900/50 transform transition-all active:scale-95 hover:-translate-y-1"
            >
              Buka Hati 🔓
            </button>
          </div>
        </div>
      </div>
    </Transition>

    <!-- OVERLAY: Curtain Reveal Effect -->
    <div v-if="isUnlocked" class="absolute inset-0 z-50 pointer-events-none">
      <div
        class="absolute top-0 left-0 w-full h-1/2 bg-[#1a0b2e] animate-curtain-up"
      ></div>
      <div
        class="absolute bottom-0 left-0 w-full h-1/2 bg-[#1a0b2e] animate-curtain-down"
      ></div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";

// Define Emits
const emit = defineEmits(['unlock']);

// State
const step = ref(1);
const inputDate = ref("");
const inputName = ref("");
const errorMsg = ref("");
const isUnlocked = ref(false);

// Generate random styles for floating orbs
const getOrbStyle = (n: number) => {
  const size = Math.random() * 100 + 50 + 'px';
  const left = Math.random() * 100 + '%';
  const top = Math.random() * 100 + '%';
  const delay = Math.random() * 5 + 's';
  const duration = (Math.random() * 10 + 10) + 's';
  
  return {
    width: size,
    height: size,
    left: left,
    top: top,
    animationDelay: delay,
    animationDuration: duration,
    background: `radial-gradient(circle, rgba(139, 92, 246, 0.3) 0%, rgba(236, 72, 153, 0) 70%)`,
    position: 'absolute' as const,
    borderRadius: '50%',
    filter: 'blur(8px)',
    animation: 'floatOrb infinite linear'
  };
};

// Logic Validasi 1
const validateDate = () => {
  const cleanDate = inputDate.value.replace(/[^0-9]/g, "");

  if (cleanDate === "13092016") {
    errorMsg.value = "";
    step.value = 2; 
  } else {
    errorMsg.value = "Salah tanggal! Hayo lupa ya? 😜";
    shakeScreen();
  }
};

// Logic Validasi 2
const validateName = () => {
  if (inputName.value.toLowerCase() === "sayang") {
    errorMsg.value = "";
    isUnlocked.value = true;

    setTimeout(() => {
      emit('unlock');
    }, 1500);
  } else {
    errorMsg.value = "Bukan itu panggilannya! Coba lagi.";
    shakeScreen();
  }
};

const shakeScreen = () => {
  const container = document.querySelector('.stage-container');
  if (container) {
    container.classList.add('animate-shake');
    setTimeout(() => {
      container.classList.remove('animate-shake');
    }, 500);
  }
};
</script>

<style>
/* Global floating animation definition */
@keyframes floatOrb {
  0% { transform: translate(0, 0) scale(1); opacity: 0.3; }
  33% { transform: translate(30px, -50px) scale(1.1); opacity: 0.6; }
  66% { transform: translate(-20px, 20px) scale(0.9); opacity: 0.3; }
  100% { transform: translate(0, 0) scale(1); opacity: 0.3; }
}

.floating-orb {
  animation-name: floatOrb;
  animation-timing-function: ease-in-out;
  animation-iteration-count: infinite;
}
</style>

<style scoped>
.fade-slide-enter-active,
.fade-slide-leave-active {
  transition: all 0.6s cubic-bezier(0.34, 1.56, 0.64, 1);
}
.fade-slide-enter-from {
  opacity: 0;
  transform: scale(0.9) translateY(20px);
}
.fade-slide-leave-to {
  opacity: 0;
  transform: scale(0.95) blur(10px);
}

@keyframes curtainUp {
  0% { transform: translateY(0); }
  100% { transform: translateY(-100%); }
}
@keyframes curtainDown {
  0% { transform: translateY(0); }
  100% { transform: translateY(100%); }
}

.animate-curtain-up {
  animation: curtainUp 1.2s cubic-bezier(0.7, 0, 0.3, 1) forwards;
}
.animate-curtain-down {
  animation: curtainDown 1.2s cubic-bezier(0.7, 0, 0.3, 1) forwards;
}

@keyframes shake {
  0%, 100% { transform: translateX(0); }
  10%, 30%, 50%, 70%, 90% { transform: translateX(-5px); }
  20%, 40%, 60%, 80% { transform: translateX(5px); }
}
.animate-shake {
  animation: shake 0.4s cubic-bezier(.36,.07,.19,.97) both;
}
</style>
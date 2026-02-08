<template>
  <div
    class="relative min-h-screen w-full overflow-hidden bg-[#0f0518] flex flex-col items-center justify-center font-sans select-none"
  >
    <!-- ========================================== -->
    <!-- 1. DYNAMIC ATMOSPHERIC BACKGROUND (LAYERED) -->
    <!-- ========================================== -->

    <!-- Layer 1: Deep Gradient -->
    <div
      class="absolute inset-0 bg-gradient-to-b from-[#1a0b2e] via-[#24123b] to-[#000000] z-0"
    ></div>

    <!-- Layer 2: Animated Nebulas (Blobs) -->
    <div
      class="absolute top-[-10%] left-[-10%] w-[600px] h-[600px] bg-purple-600/20 rounded-full blur-[120px] animate-float-slow mix-blend-screen"
    ></div>
    <div
      class="absolute bottom-[-10%] right-[-10%] w-[500px] h-[500px] bg-pink-600/10 rounded-full blur-[100px] animate-float-delayed mix-blend-screen"
    ></div>
    <div
      class="absolute top-[40%] left-[50%] -translate-x-1/2 w-[300px] h-[300px] bg-blue-500/10 rounded-full blur-[80px] animate-pulse-slow"
    ></div>

    <!-- Layer 3: Grid Pattern Overlay (Tech/Modern Feel) -->
    <div
      class="absolute inset-0 bg-[url('https://www.transparenttextures.com/patterns/cubes.png')] opacity-[0.03] z-0"
    ></div>

    <!-- ========================================== -->
    <!-- 2. MAIN INTERFACE AREA -->
    <!-- ========================================== -->
    <div class="relative z-10 w-full max-w-xl px-6 flex flex-col items-center">
      <!-- HEADER: Title & Progress -->
      <Transition name="fade-down" appear>
        <div v-if="!isFinished" class="w-full mb-8 text-center">
          <h2
            class="text-3xl md:text-4xl font-serif text-transparent bg-clip-text bg-gradient-to-r from-pink-200 via-purple-100 to-indigo-200 drop-shadow-[0_2px_10px_rgba(255,255,255,0.3)] mb-2"
          >
            Memory Lane
          </h2>
          <p
            class="text-[10px] md:text-xs font-bold tracking-[0.3em] text-purple-300/70 uppercase mb-6"
          >
            Unlock The Chapters
          </p>

          <!-- PROGRESS BAR (Elegant) -->
          <div
            class="relative w-full h-1.5 bg-white/10 rounded-full overflow-hidden"
          >
            <div
              class="absolute top-0 left-0 h-full bg-gradient-to-r from-pink-500 via-purple-500 to-indigo-500 shadow-[0_0_10px_rgba(168,85,247,0.8)] transition-all duration-700 ease-out"
              :style="{ width: progressPercentage + '%' }"
            ></div>
          </div>
          <div
            class="flex justify-between mt-2 text-[10px] text-gray-400 font-mono"
          >
            <span>START</span>
            <span>{{ currentQuestionIndex + 1 }} / {{ questions.length }}</span>
            <span>GOAL</span>
          </div>
        </div>
      </Transition>

      <!-- QUIZ CONTAINER -->
      <div class="relative w-full min-h-[400px]">
        <Transition name="switch-card" mode="out-in">
          <!-- STATE: QUIZ CARD -->
          <div
            v-if="!isFinished"
            :key="currentQuestionIndex"
            class="w-full bg-white/5 backdrop-blur-xl border border-white/10 rounded-3xl p-6 md:p-8 shadow-[0_20px_50px_-12px_rgba(0,0,0,0.5)] relative group overflow-hidden"
          >
            <!-- Decorative Glow Line Top -->
            <div
              class="absolute top-0 left-0 w-full h-1 bg-gradient-to-r from-transparent via-white/20 to-transparent opacity-50"
            ></div>

            <!-- Question Number Badge -->
            <div
              class="inline-flex items-center gap-2 bg-black/30 border border-white/10 rounded-full px-3 py-1 mb-6"
            >
              <span
                class="w-2 h-2 rounded-full bg-pink-500 animate-pulse"
              ></span>
              <span
                class="text-[10px] uppercase tracking-widest text-pink-200 font-bold"
                >Question 0{{ currentQuestionIndex + 1 }}</span
              >
            </div>

            <!-- THE QUESTION -->
            <h3
              class="text-xl md:text-2xl text-white font-medium leading-relaxed mb-8 min-h-[4rem]"
            >
              {{ currentQuestion.question }}
            </h3>

            <!-- OPTIONS GRID -->
            <div class="grid grid-cols-1 md:grid-cols-2 gap-3 md:gap-4">
              <button
                v-for="(option, idx) in currentQuestion.options"
                :key="idx"
                @click="handleAnswer(option, idx)"
                :disabled="isProcessing"
                class="relative overflow-hidden rounded-xl p-4 text-left border border-white/5 bg-gradient-to-br from-white/5 to-transparent hover:bg-white/10 hover:border-purple-400/30 transition-all duration-300 group/btn active:scale-95 disabled:opacity-50 disabled:cursor-not-allowed"
                :class="{
                  '!bg-green-500/20 !border-green-500':
                    showResult && option === currentQuestion.correctAnswer,
                  '!bg-red-500/20 !border-red-500':
                    showResult &&
                    selectedIdx === idx &&
                    option !== currentQuestion.correctAnswer,
                }"
              >
                <!-- Hover Glow Effect -->
                <div
                  class="absolute inset-0 bg-gradient-to-r from-purple-500/0 via-purple-500/10 to-transparent opacity-0 group-hover/btn:opacity-100 transition-opacity duration-500"
                ></div>

                <span
                  class="relative z-10 text-sm md:text-base text-gray-200 group-hover/btn:text-white font-medium tracking-wide block text-center md:text-left"
                >
                  {{ option }}
                </span>
              </button>
            </div>

            <!-- Decorative Bottom Shine -->
            <div
              class="absolute -bottom-20 -right-20 w-40 h-40 bg-purple-500/20 rounded-full blur-[50px] pointer-events-none"
            ></div>
          </div>

          <!-- STATE: COMPLETED SUCCESS -->
          <div
            v-else
            key="finished"
            class="flex flex-col items-center justify-center text-center py-10 animate-scale-up"
          >
            <div class="relative mb-6">
              <div
                class="absolute inset-0 bg-green-500 blur-[40px] opacity-20 animate-pulse"
              ></div>
              <div
                class="relative text-6xl md:text-7xl filter drop-shadow-lg animate-bounce-slow"
              >
                ✨
              </div>
            </div>

            <h3 class="text-3xl md:text-4xl font-serif text-white mb-2">
              Perfect!
            </h3>
            <p
              class="text-purple-200/80 text-sm tracking-widest uppercase mb-8"
            >
              All Memories Unlocked
            </p>

            <div
              class="w-12 h-12 border-4 border-purple-500 border-t-transparent rounded-full animate-spin"
            ></div>
            <p class="mt-4 text-xs text-gray-400">Loading Reward...</p>
          </div>
        </Transition>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from "vue";

// Definisi Emits
const emit = defineEmits(["complete"]);

// === DATA QUIZ (Silakan Edit Di Sini) ===
const questions = [
  {
    question: "Apa makanan kesukaan iam?",
    options: ["Padang Upik", "Sarden", "Nasi Mawut Arema", "Pecel"],
    correctAnswer: "Sarden",
  },
  {
    question: "Apa warna kesukaan iam",
    options: ["Hitam", "Navy", "Orange", "Semuanya"],
    correctAnswer: "Semuanya",
  },
  {
    question: "Tempat impian iam buat kita liburan nanti?",
    options: ["Mekkah", "Turki", "Paris", "Ranjang Tidur"],
    correctAnswer: "Turki",
  },
  {
    question: "Hal apa yang paling iam gak suka?",
    options: ["Dibohongin", "Ditelantarin", "Dicuekin", "Semuanya"],
    correctAnswer: "Semuanya",
  },
];

// === STATE MANAGEMENT ===
const currentQuestionIndex = ref(0);
const isProcessing = ref(false);
const showResult = ref(false);
const selectedIdx = ref<number | null>(null);
const isFinished = ref(false);

// Computed
const currentQuestion = computed(() => questions[currentQuestionIndex.value]);
const progressPercentage = computed(
  () => (currentQuestionIndex.value / questions.length) * 100,
);

// === LOGIC ===
const handleAnswer = (option: string, index: number) => {
  if (isProcessing.value) return; // Prevent double click

  isProcessing.value = true;
  selectedIdx.value = index;
  showResult.value = true; // Trigger color change (Green/Red)

  const isCorrect = option === currentQuestion.value.correctAnswer;

  if (isCorrect) {
    // Correct Answer Logic
    setTimeout(() => {
      if (currentQuestionIndex.value < questions.length - 1) {
        // Next Question
        currentQuestionIndex.value++;
        resetState();
      } else {
        // Finish
        finishStage();
      }
    }, 1000); // Delay 1 detik untuk user lihat warna hijau
  } else {
    // Wrong Answer Logic
    triggerShake();
    setTimeout(() => {
      showResult.value = false;
      isProcessing.value = false;
      selectedIdx.value = null;
    }, 800);
  }
};

const resetState = () => {
  showResult.value = false;
  isProcessing.value = false;
  selectedIdx.value = null;
};

const finishStage = () => {
  isFinished.value = true;
  setTimeout(() => {
    emit("complete");
  }, 3000); // 3 Detik nampilin "Perfect" screen sebelum pindah
};

const triggerShake = () => {
  const container = document.querySelector(".group");
  if (container) {
    container.classList.add("animate-shake");
    setTimeout(() => container.classList.remove("animate-shake"), 400);
  }
};
</script>

<style scoped>
/* --- ANIMATIONS --- */

/* 1. Floating Background Blobs */
@keyframes float-slow {
  0%,
  100% {
    transform: translate(0, 0) scale(1);
  }
  50% {
    transform: translate(30px, -50px) scale(1.1);
  }
}
@keyframes float-delayed {
  0%,
  100% {
    transform: translate(0, 0) scale(1);
  }
  50% {
    transform: translate(-30px, 30px) scale(0.9);
  }
}
.animate-float-slow {
  animation: float-slow 15s ease-in-out infinite;
}
.animate-float-delayed {
  animation: float-delayed 18s ease-in-out infinite reverse;
}

/* 2. Transition: Switch Card (Slide & Fade) */
.switch-card-enter-active,
.switch-card-leave-active {
  transition: all 0.5s cubic-bezier(0.4, 0, 0.2, 1);
}
.switch-card-enter-from {
  opacity: 0;
  transform: translateX(50px) scale(0.95);
  filter: blur(10px);
}
.switch-card-leave-to {
  opacity: 0;
  transform: translateX(-50px) scale(1.05);
  filter: blur(10px);
}

/* 3. Transition: Fade Down (Header) */
.fade-down-enter-active,
.fade-down-leave-active {
  transition: all 0.8s ease;
}
.fade-down-enter-from {
  opacity: 0;
  transform: translateY(-20px);
}
.fade-down-leave-to {
  opacity: 0;
  transform: translateY(-20px);
}

/* 4. Shake Error Effect */
@keyframes shake {
  0%,
  100% {
    transform: translateX(0);
  }
  20%,
  60% {
    transform: translateX(-5px);
  }
  40%,
  80% {
    transform: translateX(5px);
  }
}
.animate-shake {
  animation: shake 0.4s cubic-bezier(0.36, 0.07, 0.19, 0.97) both;
}

/* 5. Scale Up Success */
.animate-scale-up {
  animation: scaleUp 0.6s cubic-bezier(0.34, 1.56, 0.64, 1);
}
@keyframes scaleUp {
  from {
    opacity: 0;
    transform: scale(0.8);
  }
  to {
    opacity: 1;
    transform: scale(1);
  }
}

.animate-bounce-slow {
  animation: bounce 3s infinite;
}
</style>

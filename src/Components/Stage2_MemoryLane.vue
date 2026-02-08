<template>
  <div class="min-h-screen flex flex-col items-center justify-center p-6 bg-gradient-to-b from-[#1a0b2e] to-[#2d1b4e] text-white">
    <!-- Header -->
    <h2 class="text-3xl font-serif text-purple-200 mb-2 drop-shadow-md animate-fade-in-down">
      Memory Lane ❤️
    </h2>
    <p class="text-sm text-purple-300/80 mb-8 animate-fade-in-up">
      Jawab pertanyaan untuk membuka gembok hati kita.
    </p>

    <!-- Locks Visualization -->
    <div class="flex gap-4 mb-8">
      <div 
        v-for="(q, index) in questions" 
        :key="index"
        class="flex flex-col items-center transition-all duration-500"
        :class="q.isUnlocked ? 'scale-110' : 'opacity-50'"
      >
        <div class="text-4xl filter drop-shadow-[0_0_10px_rgba(236,72,153,0.8)]">
          {{ q.isUnlocked ? '🔓' : '🔒' }}
        </div>
        <div class="w-2 h-2 rounded-full mt-2" :class="q.isUnlocked ? 'bg-green-400 shadow-[0_0_8px_rgba(74,222,128,1)]' : 'bg-red-500'"></div>
      </div>
    </div>

    <!-- Quiz Card -->
    <Transition name="slide-up" mode="out-in">
      <div 
        v-if="currentQuestionIndex < questions.length" 
        :key="currentQuestionIndex"
        class="w-full max-w-sm bg-white/10 backdrop-blur-lg border border-white/20 p-6 rounded-2xl shadow-xl relative overflow-hidden"
      >
        <!-- Progress Bar -->
        <div class="absolute top-0 left-0 h-1 bg-gradient-to-r from-purple-500 to-pink-500 transition-all duration-300" :style="{ width: ((currentQuestionIndex) / questions.length) * 100 + '%' }"></div>

        <h3 class="text-xl font-medium mb-4 text-purple-100">
          {{ questions[currentQuestionIndex].question }}
        </h3>
        
        <div class="grid grid-cols-1 gap-3">
          <button
            v-for="(option, idx) in questions[currentQuestionIndex].options"
            :key="idx"
            @click="handleAnswer(option)"
            class="py-3 px-4 rounded-xl text-left transition-all duration-200 border border-white/10 hover:border-purple-400 hover:bg-purple-500/20 active:scale-98 flex justify-between items-center group"
          >
            <span>{{ option }}</span>
            <span class="opacity-0 group-hover:opacity-100 transition-opacity">✨</span>
          </button>
        </div>
      </div>

      <!-- Completion Message (Briefly shown before transition) -->
      <div v-else class="text-center animate-bounce">
        <h3 class="text-2xl font-bold text-pink-300">Semua Gembok Terbuka! 🗝️</h3>
        <p class="text-purple-200">Menuju hadiah utama...</p>
      </div>
    </Transition>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const emit = defineEmits(['complete']);

interface Question {
  id: number;
  question: string;
  options: string[];
  correctAnswer: string;
  isUnlocked: boolean;
}

// DATA KUIS - SILAHKAN GANTI DI SINI
const questions = ref<Question[]>([
  {
    id: 1,
    question: "Di mana kita pertama kali makan bareng?",
    options: ["KFC", "Warung Tegal", "Sushi Tei", "Nasi Goreng Pinggir Jalan"],
    correctAnswer: "KFC", // Ganti dengan jawaban benar
    isUnlocked: false,
  },
  {
    id: 2,
    question: "Apa warna kesukaan aku?",
    options: ["Merah", "Ungu", "Hitam", "Biru Langit"],
    correctAnswer: "Ungu",
    isUnlocked: false,
  },
  {
    id: 3,
    question: "Siapa yang paling ngangenin?",
    options: ["Kamu", "Aku", "Tetangga", "Kucing"],
    correctAnswer: "Kamu",
    isUnlocked: false,
  }
]);

const currentQuestionIndex = ref(0);

// Audio Effect
const playSound = (type: 'correct' | 'wrong') => {
  // Pastikan file audio ada atau gunakan AudioContext simple
  // Disini kita simulasi saja atau gunakan path aset jika ada
  // const audio = new Audio(type === 'correct' ? '/assets/correct.mp3' : '/assets/wrong.mp3');
  // audio.play().catch(() => {});
};

const handleAnswer = (selectedOption: string) => {
  const currentQ = questions.value[currentQuestionIndex.value];
  
  if (selectedOption === currentQ.correctAnswer) {
    // Jawaban Benar
    currentQ.isUnlocked = true;
    playSound('correct');
    
    // Delay sedikit biar user lihat feedback
    setTimeout(() => {
      currentQuestionIndex.value++;
      
      // Cek apakah sudah selesai semua
      if (currentQuestionIndex.value === questions.value.length) {
        setTimeout(() => {
          emit('complete');
        }, 1000);
      }
    }, 500);
  } else {
    // Jawaban Salah
    playSound('wrong');
    // Bisa tambahkan animasi getar di sini
    alert("Salah weyy! 🤣 Coba lagi!");
  }
};
</script>

<style scoped>
/* Animasi Slide Up untuk Kartu Soal */
.slide-up-enter-active,
.slide-up-leave-active {
  transition: all 0.5s ease;
}
.slide-up-enter-from {
  opacity: 0;
  transform: translateY(30px);
}
.slide-up-leave-to {
  opacity: 0;
  transform: translateY(-30px);
}

.animate-fade-in-down {
  animation: fadeInDown 1s ease-out;
}
@keyframes fadeInDown {
  from { opacity: 0; transform: translateY(-20px); }
  to { opacity: 1; transform: translateY(0); }
}

.animate-fade-in-up {
  animation: fadeInUp 1s ease-out 0.3s backwards;
}
@keyframes fadeInUp {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}
</style>
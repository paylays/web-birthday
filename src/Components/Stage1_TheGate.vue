<template>
  <div
    class="stage-container relative w-full h-screen overflow-hidden bg-[#1a0b2e] flex flex-col items-center justify-center text-white"
  >
    <!-- BACKGROUND: 3D Spline Scene (Orb Ungu) -->
    <div class="absolute inset-0 z-0 opacity-60">
      <!-- Ganti URL ini dengan scene Spline buatanmu atau gunakan placeholder ini -->
      <Spline
        :scene="'https://prod.spline.design/6Wq1Q7YGyM-iab9i/scene.splinecode'"
      />
    </div>

    <!-- CONTENT: Login Form -->
    <Transition name="fade-slide" mode="out-in">
      <div
        v-if="!isUnlocked"
        class="z-10 w-4/5 max-w-sm backdrop-blur-md bg-white/5 border border-white/10 p-8 rounded-2xl shadow-[0_0_30px_rgba(139,92,246,0.3)]"
      >
        <h1
          class="text-3xl font-serif text-center mb-2 text-purple-200 drop-shadow-[0_0_8px_rgba(216,180,254,0.8)]"
        >
          Welcome Home
        </h1>
        <p
          class="text-xs text-center text-gray-300 mb-8 font-light tracking-widest"
        >
          A SPECIAL PLACE FOR US
        </p>

        <!-- QUESTION 1 -->
        <div v-if="step === 1" class="space-y-4">
          <label class="block text-sm font-medium text-purple-300"
            >Kapan kita jadian?</label
          >
          <input
            v-model="inputDate"
            type="text"
            placeholder="DDMMYYYY"
            class="w-full bg-black/30 border border-purple-500/30 rounded-lg px-4 py-3 text-center text-lg tracking-widest focus:outline-none focus:border-purple-400 focus:shadow-[0_0_15px_rgba(168,85,247,0.5)] transition-all duration-300 placeholder-gray-600"
            @keyup.enter="validateDate"
          />
          <p
            v-if="errorMsg"
            class="text-red-400 text-xs text-center animate-pulse"
          >
            {{ errorMsg }}
          </p>
          <button
            @click="validateDate"
            class="w-full bg-gradient-to-r from-purple-600 to-indigo-600 py-2 rounded-lg font-semibold shadow-lg hover:shadow-purple-500/50 transition-all active:scale-95"
          >
            Jawab ❤️
          </button>
        </div>

        <!-- QUESTION 2 -->
        <div v-else-if="step === 2" class="space-y-4">
          <label class="block text-sm font-medium text-purple-300"
            >Panggilan sayang aku ke kamu?</label
          >
          <input
            v-model="inputName"
            type="text"
            placeholder="???"
            class="w-full bg-black/30 border border-purple-500/30 rounded-lg px-4 py-3 text-center text-lg font-bold tracking-widest focus:outline-none focus:border-pink-400 focus:shadow-[0_0_15px_rgba(236,72,153,0.5)] transition-all duration-300 uppercase"
            @keyup.enter="validateName"
          />
          <p
            v-if="errorMsg"
            class="text-red-400 text-xs text-center animate-pulse"
          >
            {{ errorMsg }}
          </p>
          <button
            @click="validateName"
            class="w-full bg-gradient-to-r from-pink-600 to-purple-600 py-2 rounded-lg font-semibold shadow-lg hover:shadow-pink-500/50 transition-all active:scale-95"
          >
            Buka Hati 🔓
          </button>
        </div>
      </div>
    </Transition>

    <!-- OVERLAY: Efek Transisi Layar Terbelah (CSS Only) -->
    <div v-if="isUnlocked" class="absolute inset-0 z-50 pointer-events-none">
      <div
        class="absolute top-0 left-0 w-full h-1/2 bg-[#1a0b2e] animate-split-up"
      ></div>
      <div
        class="absolute bottom-0 left-0 w-full h-1/2 bg-[#1a0b2e] animate-split-down"
      ></div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import Spline from "spline-vue/v3";

// Define Emits
const emit = defineEmits(["unlock"]);

// State
const step = ref(1);
const inputDate = ref("");
const inputName = ref("");
const errorMsg = ref("");
const isUnlocked = ref(false);

// Logic Validasi 1
const validateDate = () => {
  // Hapus separator user kalau ada
  const cleanDate = inputDate.value.replace(/[^0-9]/g, "");

  // GANTI TANGGAL JADIAN DI SINI (Format: DDMMYYYY)
  // Contoh: 13092016
  if (cleanDate === "13092016") {
    errorMsg.value = "";
    step.value = 2; // Lanjut ke pertanyaan kedua
  } else {
    errorMsg.value = "Hayo lupa ya? 😜 Coba ingat lagi!";
    shakeScreen();
  }
};

// Logic Validasi 2
const validateName = () => {
  // GANTI PANGGILAN SAYANG DI SINI (Huruf kecil semua)
  if (inputName.value.toLowerCase() === "sayang") {
    errorMsg.value = "";
    isUnlocked.value = true;

    // Emit event ke Parent Component untuk ganti ke Stage 2 setelah animasi selesai (1.5s)
    setTimeout(() => {
      emit("unlock");
    }, 1500);
  } else {
    errorMsg.value = "Salah sayang, coba ingat nama panggilannya!";
    shakeScreen();
  }
};

const shakeScreen = () => {
  const container = document.querySelector(".stage-container");
  if (container) {
    container.classList.add("animate-shake");
    setTimeout(() => {
      container.classList.remove("animate-shake");
    }, 500);
  }
};
</script>

<style scoped>
/* Transisi Masuk/Keluar Form */
.fade-slide-enter-active,
.fade-slide-leave-active {
  transition: all 0.5s ease;
}
.fade-slide-enter-from {
  opacity: 0;
  transform: translateY(20px);
}
.fade-slide-leave-to {
  opacity: 0;
  transform: translateY(-20px);
}

/* Animasi Layar Terbelah (Split Screen) */
@keyframes splitUp {
  0% {
    transform: translateY(0);
  }
  100% {
    transform: translateY(-100%);
  }
}
@keyframes splitDown {
  0% {
    transform: translateY(0);
  }
  100% {
    transform: translateY(100%);
  }
}

.animate-split-up {
  animation: splitUp 1.5s cubic-bezier(0.7, 0, 0.3, 1) forwards;
}
.animate-split-down {
  animation: splitDown 1.5s cubic-bezier(0.7, 0, 0.3, 1) forwards;
}

/* Animasi Shake untuk Error */
@keyframes shake {
  0%,
  100% {
    transform: translateX(0);
  }
  10%,
  30%,
  50%,
  70%,
  90% {
    transform: translateX(-5px);
  }
  20%,
  40%,
  60%,
  80% {
    transform: translateX(5px);
  }
}
.animate-shake {
  animation: shake 0.5s cubic-bezier(0.36, 0.07, 0.19, 0.97) both;
}
</style>

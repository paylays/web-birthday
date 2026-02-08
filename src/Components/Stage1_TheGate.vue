<template>
  <div
    class="stage-container relative w-full h-screen overflow-hidden bg-[#0f0518] flex flex-col items-center justify-center text-white"
  >
    <!-- BACKGROUND: Dynamic Aurora & Stars -->
    <div class="absolute inset-0 z-0">
      <div
        class="absolute inset-0 bg-gradient-to-b from-[#1a0b2e] via-[#2d1b4e] to-[#000000]"
      ></div>
      <!-- Animated Aurora Blobs -->
      <div
        class="absolute top-[-10%] left-[-10%] w-[500px] h-[500px] bg-purple-600/30 rounded-full blur-[100px] animate-blob"
      ></div>
      <div
        class="absolute bottom-[-10%] right-[-10%] w-[500px] h-[500px] bg-pink-600/20 rounded-full blur-[100px] animate-blob animation-delay-2000"
      ></div>
      <div
        class="absolute top-[40%] left-[50%] transform -translate-x-1/2 w-[300px] h-[300px] bg-blue-500/20 rounded-full blur-[100px] animate-blob animation-delay-4000"
      ></div>

      <!-- Stars (CSS generated) -->
      <div class="stars"></div>
      <div class="stars2"></div>
    </div>

    <!-- CONTENT: Glass Card -->
    <Transition name="zoom-fade" mode="out-in" appear>
      <div
        v-if="!isUnlocked"
        class="z-10 w-[90%] max-w-md relative group perspective-1000"
      >
        <!-- Glowing Border Gradient -->
        <div
          class="absolute -inset-0.5 bg-gradient-to-r from-pink-500 via-purple-500 to-indigo-500 rounded-3xl opacity-50 blur group-hover:opacity-100 transition duration-1000 animate-tilt"
        ></div>

        <!-- Main Card Content -->
        <div
          class="relative bg-black/40 backdrop-blur-2xl border border-white/10 rounded-3xl p-8 shadow-2xl overflow-hidden"
        >
          <!-- Header -->
          <div class="text-center mb-10 space-y-2">
            <h1
              class="text-5xl font-serif text-transparent bg-clip-text bg-gradient-to-br from-white via-pink-200 to-purple-200 drop-shadow-[0_0_15px_rgba(255,255,255,0.3)] animate-title-float"
            >
              Welcome Home
            </h1>
            <div
              class="h-1 w-20 mx-auto bg-gradient-to-r from-transparent via-purple-500 to-transparent rounded-full opacity-70"
            ></div>
            <p
              class="text-xs text-purple-200/70 tracking-[0.4em] uppercase font-light"
            >
              Enter The Secret Place
            </p>
          </div>

          <!-- INPUT FORM WRAPPER -->
          <div class="relative min-h-[200px]">
            <!-- QUESTION 1 -->
            <Transition name="slide-left" mode="out-in">
              <div v-if="step === 1" key="step1" class="space-y-6">
                <div class="relative group/input">
                  <label
                    class="block text-xs font-medium text-pink-300 mb-2 ml-1 tracking-wider uppercase"
                  >
                    Kapan kita jadian?
                  </label>
                  <input
                    v-model="inputDate"
                    type="text"
                    placeholder="DDMMYYYY"
                    class="w-full bg-white/5 border border-white/10 rounded-xl px-4 py-4 text-center text-2xl tracking-[0.5em] text-white placeholder-white/20 focus:outline-none focus:bg-white/10 focus:border-pink-500/50 focus:ring-4 focus:ring-pink-500/20 transition-all duration-300 font-mono"
                    @keyup.enter="validateDate"
                  />
                  <!-- Error Message -->
                  <p
                    v-if="errorMsg"
                    class="absolute -bottom-6 left-0 w-full text-center text-red-400 text-xs font-bold animate-pulse"
                  >
                    {{ errorMsg }}
                  </p>
                </div>

                <button
                  @click="validateDate"
                  class="w-full group relative overflow-hidden bg-gradient-to-r from-purple-600 to-pink-600 text-white py-4 rounded-xl font-bold tracking-widest shadow-lg transform transition-all hover:scale-[1.02] active:scale-95"
                >
                  <span
                    class="absolute inset-0 bg-white/20 translate-y-full group-hover:translate-y-0 transition-transform duration-300"
                  ></span>
                  <span class="relative flex items-center justify-center gap-2">
                    LANJUT
                    <span class="group-hover:translate-x-1 transition-transform"
                      >→</span
                    >
                  </span>
                </button>
              </div>

              <!-- QUESTION 2 -->
              <div v-else-if="step === 2" key="step2" class="space-y-6">
                <div class="relative group/input">
                  <label
                    class="block text-xs font-medium text-purple-300 mb-2 ml-1 tracking-wider uppercase"
                  >
                    Panggilan cowo terganteng di hidupmu?
                  </label>
                  <input
                    v-model="inputName"
                    type="text"
                    placeholder="..."
                    class="w-full bg-white/5 border border-white/10 rounded-xl px-4 py-4 text-center text-2xl font-bold tracking-widest text-white placeholder-white/20 focus:outline-none focus:bg-white/10 focus:border-purple-500/50 focus:ring-4 focus:ring-purple-500/20 transition-all duration-300 uppercase"
                    @keyup.enter="validateName"
                  />
                  <p
                    v-if="errorMsg"
                    class="absolute -bottom-6 left-0 w-full text-center text-red-400 text-xs font-bold animate-pulse"
                  >
                    {{ errorMsg }}
                  </p>
                </div>

                <button
                  @click="validateName"
                  class="w-full group relative overflow-hidden bg-gradient-to-r from-pink-600 to-purple-600 text-white py-4 rounded-xl font-bold tracking-widest shadow-lg transform transition-all hover:scale-[1.02] active:scale-95"
                >
                  <span
                    class="absolute inset-0 bg-white/20 translate-y-full group-hover:translate-y-0 transition-transform duration-300"
                  ></span>
                  <span class="relative flex items-center justify-center gap-2">
                    BUKA HATI 🔓
                  </span>
                </button>
              </div>
            </Transition>
          </div>
        </div>
      </div>
    </Transition>

    <!-- CURTAIN REVEAL -->
    <div
      v-if="isUnlocked"
      class="absolute inset-0 z-50 pointer-events-none flex flex-col"
    >
      <div
        class="h-1/2 w-full bg-[#1a0b2e] animate-curtain-up border-b-2 border-purple-500/50 shadow-[0_0_50px_rgba(168,85,247,0.5)]"
      ></div>
      <div
        class="h-1/2 w-full bg-[#1a0b2e] animate-curtain-down border-t-2 border-purple-500/50 shadow-[0_0_50px_rgba(168,85,247,0.5)]"
      ></div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";

const emit = defineEmits(["unlock"]);

// State
const step = ref(1);
const inputDate = ref("");
const inputName = ref("");
const errorMsg = ref("");
const isUnlocked = ref(false);

// Logic Validasi (Sama seperti sebelumnya)
const validateDate = () => {
  const cleanDate = inputDate.value.replace(/[^0-9]/g, "");
  // GANTI TANGGAL DI SINI
  if (cleanDate === "13092016") {
    errorMsg.value = "";
    step.value = 2;
  } else {
    triggerError("Salah tanggal! Hayo lupa ya? 😜");
  }
};

const validateName = () => {
  // GANTI NAMA PANGGILAN DI SINI
  if (inputName.value.toLowerCase() === "iam") {
    errorMsg.value = "";
    isUnlocked.value = true;
    setTimeout(() => {
      emit("unlock");
    }, 1500);
  } else {
    triggerError("Bukan itu sayang panggilannya! Coba lagi deh.");
  }
};

const triggerError = (msg: string) => {
  errorMsg.value = msg;
  // Efek getar sederhana
  const card = document.querySelector(".group");
  if (card) {
    card.classList.add("animate-shake");
    setTimeout(() => card.classList.remove("animate-shake"), 500);
  }
};
</script>

<style scoped>
/* 1. ANIMASI BACKGROUND BLOB */
@keyframes blob {
  0% {
    transform: translate(0px, 0px) scale(1);
  }
  33% {
    transform: translate(30px, -50px) scale(1.1);
  }
  66% {
    transform: translate(-20px, 20px) scale(0.9);
  }
  100% {
    transform: translate(0px, 0px) scale(1);
  }
}
.animate-blob {
  animation: blob 7s infinite;
}
.animation-delay-2000 {
  animation-delay: 2s;
}
.animation-delay-4000 {
  animation-delay: 4s;
}

/* 2. STARS EFFECT (CSS ONLY) */
.stars {
  width: 1px;
  height: 1px;
  background: transparent;
  box-shadow:
    100px 200px #fff,
    400px 300px #fff,
    700px 100px #fff,
    200px 500px #fff,
    600px 400px #fff,
    900px 200px #fff;
  animation: animStar 50s linear infinite;
}
.stars2 {
  width: 2px;
  height: 2px;
  background: transparent;
  box-shadow:
    150px 250px #fff,
    450px 350px #fff,
    750px 150px #fff;
  animation: animStar 100s linear infinite;
}
@keyframes animStar {
  from {
    transform: translateY(0px);
  }
  to {
    transform: translateY(-2000px);
  }
}

/* 3. TILT GLOW EFFECT */
.animate-tilt {
  animation: tilt 10s infinite linear;
}
@keyframes tilt {
  0%,
  50%,
  100% {
    transform: rotate(0deg);
  }
  25% {
    transform: rotate(1deg);
  }
  75% {
    transform: rotate(-1deg);
  }
}

/* 4. TRANSITIONS */
/* Zoom Fade untuk Card Masuk */
.zoom-fade-enter-active {
  transition: all 1s ease-out;
}
.zoom-fade-leave-active {
  transition: all 0.5s ease-in;
}
.zoom-fade-enter-from {
  opacity: 0;
  transform: scale(0.9) translateY(50px);
}
.zoom-fade-leave-to {
  opacity: 0;
  transform: scale(1.1) blur(10px);
}

/* Slide Left untuk Pergantian Pertanyaan */
.slide-left-enter-active,
.slide-left-leave-active {
  transition: all 0.4s ease;
}
.slide-left-enter-from {
  opacity: 0;
  transform: translateX(20px);
}
.slide-left-leave-to {
  opacity: 0;
  transform: translateX(-20px);
}

/* Curtain Animation */
@keyframes curtainUp {
  to {
    transform: translateY(-100%);
  }
}
.animate-curtain-up {
  animation: curtainUp 1.5s cubic-bezier(0.8, 0, 0.2, 1) forwards;
}
@keyframes curtainDown {
  to {
    transform: translateY(100%);
  }
}
.animate-curtain-down {
  animation: curtainDown 1.5s cubic-bezier(0.8, 0, 0.2, 1) forwards;
}

/* Shake Error */
@keyframes shake {
  0%,
  100% {
    transform: translateX(0);
  }
  25% {
    transform: translateX(-5px);
  }
  75% {
    transform: translateX(5px);
  }
}
.animate-shake {
  animation: shake 0.3s ease-in-out;
}
</style>

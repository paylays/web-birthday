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

          <p class="font-serif text-gray-600 mb-6 leading-relaxed">
            Karena cindy udah jadi pacar yang hebat dan berhasil lewatin semua
            tantangan tadi, ini hadiah kecil buat cindy:
          </p>

          <!-- COUPON AREA -->
          <div
            class="bg-red-50 border-2 border-dashed border-red-300 p-6 rounded-xl relative group hover:bg-red-100 transition-colors cursor-default mb-8"
          >
            <div
              class="absolute -left-3 top-1/2 -translate-y-1/2 w-6 h-6 bg-[#fff9f0] rounded-full border-r-2 border-red-300"
            ></div>
            <div
              class="absolute -right-3 top-1/2 -translate-y-1/2 w-6 h-6 bg-[#fff9f0] rounded-full border-l-2 border-red-300"
            ></div>

            <h3 class="text-2xl font-bold text-red-600 mb-1">FREE MAEMM</h3>
            <p class="text-sm text-red-400 uppercase tracking-widest">
              Anywhere You Want
            </p>
          </div>

          <!-- WHATSAPP BUTTON (NEW) -->
          <a
            :href="whatsappLink"
            target="_blank"
            class="flex items-center justify-center gap-2 w-full bg-[#25D366] hover:bg-[#128C7E] text-white font-bold py-3 px-6 rounded-full shadow-lg transform transition-all hover:scale-105 hover:-translate-y-1 mb-4 group"
          >
            <!-- WhatsApp Icon SVG -->
            <svg
              class="w-6 h-6 fill-current"
              viewBox="0 0 24 24"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                d="M17.472 14.382c-.297-.149-1.758-.867-2.03-.967-.273-.099-.471-.148-.67.15-.197.297-.767.966-.94 1.164-.173.199-.347.223-.644.075-.297-.15-1.255-.463-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.298-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.075-.149-.669-1.612-.916-2.207-.242-.579-.487-.5-.669-.51-.173-.008-.371-.01-.57-.01-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.096 3.2 5.077 4.487.709.306 1.262.489 1.694.625.712.227 1.36.195 1.871.118.571-.085 1.758-.719 2.006-1.413.248-.694.248-1.289.173-1.413-.074-.124-.272-.198-.57-.347m-5.421 7.403h-.004a9.87 9.87 0 01-5.031-1.378l-.361-.214-3.741.982.998-3.648-.235-.374a9.86 9.86 0 01-1.51-5.26c.001-5.45 4.436-9.884 9.888-9.884 2.64 0 5.122 1.03 6.988 2.898a9.825 9.825 0 012.893 6.994c-.003 5.45-4.437 9.884-9.885 9.884m8.413-18.297A11.815 11.815 0 0012.05 0C5.495 0 .16 5.335.157 11.892c0 2.096.547 4.142 1.588 5.945L.057 24l6.305-1.654a11.882 11.882 0 005.683 1.448h.005c6.554 0 11.89-5.335 11.893-11.893a11.821 11.821 0 00-3.48-8.413Z"
              />
            </svg>
            <span>Klaim di WhatsApp</span>
          </a>

          <p class="mt-4 text-xs text-gray-400 font-mono">
            *Klik tombol di atas yeeee!!!!!!
          </p>

          <!-- Close / Reset -->
          <button
            @click="reloadPage"
            class="mt-6 text-gray-400 hover:text-red-500 text-xs transition-colors flex items-center justify-center gap-1 mx-auto"
          >
            <span>↺</span> Ulangi Dari Awal
          </button>
        </div>
      </div>
    </Transition>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from "vue";

const isOpen = ref(false);

// NOMOR WHATSAPP & PESAN (Otomatis encode URL)
const phoneNumber = "6285387432115"; // Gunakan format 62 (bukan 08)
const message = "seriuss yamm??? iihhh cindy bingunggg mau maem dimanaaa";
const whatsappLink = computed(() => {
  return `https://wa.me/${phoneNumber}?text=${encodeURIComponent(message)}`;
});

const openGift = () => {
  isOpen.value = true;
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

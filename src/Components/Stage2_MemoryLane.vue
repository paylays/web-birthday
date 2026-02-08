<template>
  <div class="h-screen flex flex-col items-center justify-center">
    <h2 class="text-3xl font-serif mb-4">Mengingat Kita...</h2>
    <p>Loading Memories...</p>
    <!-- Nanti kita isi kuis di sini -->
    <button
      @click="$emit('complete')"
      class="mt-4 px-4 py-2 bg-purple-600 rounded"
    >
      (Tombol Debug: Skip ke Stage 3)
    </button>
  </div>
</template>

<script setup lang="ts">
defineEmits(["complete"]);
</script>

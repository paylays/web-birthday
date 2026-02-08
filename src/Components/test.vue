<script setup lang="ts">
import { ref, computed } from "vue";

// Definisi Emits
const emit = defineEmits(["complete"]);

// Definisi Tipe Data agar lebih aman
interface Question {
  question: string;
  options: string[];
  correctAnswer: string;
}

// === DATA QUIZ ===
const questions: Question[] = [
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

// Computed: Tambahkan fallback "|| {}" atau assertion "!"
const currentQuestion = computed(() => {
  return questions[currentQuestionIndex.value];
});

// Progress Calculation
const progressPercentage = computed(() => {
  return (currentQuestionIndex.value / questions.length) * 100;
});

// === LOGIC ===
const handleAnswer = (option: string, index: number) => {
  if (isProcessing.value) return;

  // Guard clause: Jika pertanyaan tidak ada (undefined), stop
  const question = currentQuestion.value;
  if (!question) return;

  isProcessing.value = true;
  selectedIdx.value = index;
  showResult.value = true;

  const isCorrect = option === question.correctAnswer;

  if (isCorrect) {
    // Correct Answer Logic
    setTimeout(() => {
      if (currentQuestionIndex.value < questions.length - 1) {
        currentQuestionIndex.value++;
        resetState();
      } else {
        finishStage();
      }
    }, 1000);
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
  }, 3000);
};

const triggerShake = () => {
  const container = document.querySelector(".group");
  if (container) {
    container.classList.add("animate-shake");
    setTimeout(() => container.classList.remove("animate-shake"), 400);
  }
};
</script>

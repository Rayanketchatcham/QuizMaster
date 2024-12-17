<script setup lang="ts">
import {
  faBars,
  faMedal,
  faStopwatch,
} from "@fortawesome/free-solid-svg-icons";
import Button from "./Components/Button.vue";
import ItemQuiz from "./Components/ItemQuiz.vue";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";
import { computed, ref } from "vue";
import { questions } from "./data/question";

let quizStarted = ref(false);
let quizEnd = ref(false);
let currentQuestionIndex = ref(0);
const select = ref("");
let score = ref(0);
const timer = ref(0);
const reStart = ref(false);
const currentQuestion = computed(() => questions[currentQuestionIndex.value]); // Récupérer la question actuelle
let intervalId: NodeJS.Timeout | null = null; // Identifiant de l'intervalle pour pouvoir l'annuler

// Fonction pour démarrer le quiz

const startQuiz = () => {
  quizStarted.value = true;
  currentQuestionIndex.value = 0;
  timer.value = 0;

  // Annuler l'intervalle précédent s'il existe
  if (intervalId) {
    clearInterval(intervalId);
  }

  intervalId = setInterval(() => {
    timer.value = timer.value + 1;
  }, 1000);
};

// Fonction pour redémarrer le quiz

const reStartQuiz = () => {
  quizStarted.value = false;
  quizEnd.value = false;
  currentQuestionIndex.value = 0;
  select.value = "";
  score.value = 0;
  reStart.value = false;

  if (intervalId) {
    clearInterval(intervalId);
  }
};

// Fonction pour passer à la question suivante

const nextQuestion = () => {
  if (select.value == currentQuestion.value.correctAnswer) {
    score.value = score.value + 1;
  }
  if (currentQuestionIndex.value <= questions.length) {
    currentQuestionIndex.value = currentQuestionIndex.value + 1;
  }
  if (currentQuestionIndex.value == questions.length) {
    quizEnd.value = true;
  }
  select.value = "";
};
</script>

<template>
  <section
    :style="{ display: 'flex', justifyContent: 'center', alignItems: 'center' }"
  >
    <!-- Rendu avant que le quiz commence -->
    <div
      :style="{
        display: 'flex',
        flexDirection: 'column',
        justifyContent: 'center',
        alignItems: 'center',
      }"
      style="
        background-color: whitesmoke;
        padding: 40px;
        border-radius: 12px;
        width: 50%;
      "
      v-if="!quizStarted || reStart"
    >
      <h1>Bienvenue sur QuizMaster</h1>
      <h2>L'application qui mettra ton esprit à l'épreuve ! 🎯</h2>
      <div style="display: flex; flex-direction: column">
        <ItemQuiz title="10" sub-title="Question a choix multiples">
          <template #icone>
            <FontAwesomeIcon
              :icon="faBars"
              style="
                font-size: 16px;
                color: green;
                background-color: white;
                border-radius: 20px;
                border: 2px solid green;
                padding: 8px;
              "
            />
          </template>
        </ItemQuiz>
        <ItemQuiz
          title="2 mins"
          sub-title="Pour repondre a toute les questions "
        >
          <template #icone>
            <FontAwesomeIcon
              :icon="faStopwatch"
              style="
                font-size: 16px;
                color: green;
                background-color: white;
                border-radius: 20px;
                border: 2px solid green;
                padding: 8px;
              "
            />
          </template>
        </ItemQuiz>
        <ItemQuiz title="80%" sub-title="Pour la Medaille">
          <template #icone>
            <FontAwesomeIcon
              :icon="faMedal"
              style="
                font-size: 16px;
                color: green;
                background-color: white;
                border-radius: 20px;
                border: 2px solid green;
                padding: 8px;
              "
            />
          </template>
        </ItemQuiz>
      </div>
      <div>
        <h1>Quelques astuces avant de commencer :</h1>
        <ul style="display: flex; flex-direction: column; gap: 10px">
          <li>Chaque bonne réponse vaut 10 points.</li>
          <li>
            Une fois le temps écoulé, ton score sera automatiquement calculé.
          </li>
          <li>Assure-toi de rester concentré : chaque seconde compte !</li>
        </ul>
      </div>
      <Button label="Commencer le quiz" @clickButton="startQuiz"></Button>
    </div>

    <!-- Rendu lorsque le quiz commence -->

    <div
      v-else-if="quizStarted && !quizEnd"
      style="
        background-color: whitesmoke;
        padding: 40px;
        border-radius: 12px;
        width: 50%;
      "
    >
      <div
        style="
          display: flex;
          flex-direction: row;
          justify-content: space-between;
          color: gray;
        "
      >
        <h3>Temps : {{ timer }} Secondes</h3>
        <h3>Question {{ currentQuestionIndex + 1 }}/{{ questions.length }}</h3>
      </div>

      <h3 style="text-align: center">{{ currentQuestion.question }}</h3>
      <div
        v-for="(option, index) in currentQuestion.options"
        :key="index"
        class="question"
      >
        <label
          :for="'option-' + index"
          :style="{
            color:
              select === option
                ? option === currentQuestion.correctAnswer
                  ? 'green'
                  : 'red'
                : 'black',
          }"
          >{{ option }}</label
        >
        <input
          type="radio"
          :value="option"
          v-model="select"
          :id="'option' + index"
        />
      </div>
      <div
        style="
          display: flex;
          align-items: center;
          justify-content: center;
          padding-top: 10px;
        "
      >
        <Button label="Question Suivante" @clickButton="nextQuestion"></Button>
      </div>
    </div>

    <!-- Rendu a la fin du quiz -->

    <div
      v-else
      style="
        background-color: whitesmoke;
        padding: 40px;
        border-radius: 12px;
        width: 50%;
        text-align: center;
      "
    >
      <h1>Resultat de votre Quiz</h1>
      <p>Votre Score est de {{ score }}</p>
      <Button label="Recommencer le Quiz" @clickButton="reStartQuiz"></Button>
    </div>
  </section>
</template>
<style>
.question {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  border: 1px solid grey;
  border-radius: 8px;
  padding: 8px 12px;
  margin: 10px;
}
</style>

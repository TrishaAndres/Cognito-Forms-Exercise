<template>
  <div>
    <pre>{{ JSON.stringify(questions, null, 2) }}</pre>
    <div v-if="currentQuestionIndex < questions.length">
      <h3>
        Questions {{ currentQuestionIndex + 1 }}:
        {{ questions[currentQuestionIndex].question }}
      </h3>
      <div
        v-for="(answer, index) in questions[currentQuestionIndex].answers"
        :key="index"
      >
        <label>
          <input
            type="radio"
            :value="answer"
            v-model="selectedAnswer"
            :disabled="answered"
          />
          {{ answer }}
        </label>
      </div>
      <button @click="submitAnswer" :disabled="!selectedAnswer || answered">
        Next
      </button>
    </div>

    <div v-else>
      <h3>Summary</h3>
      <ul>
        <li v-for="(response, index) in userAnswers" :key="index">
          {{ index + 1 }}, {{ response.question }} - Your Answer:
          {{ response.answer }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    questions: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      currentQuestionIndex: 0,
      selectedAnswer: null,
      answered: false,
      userAnswers: [],
    };
  },
  methods: {
    submitAnswer() {
      if (this.selectedAnswer) {
        this.userAnswers.push({
          question: this.questions[this.currentQuestionIndex].text,
          answer: this.selectedAnswer,
        });

        this.currentQuestionIndex++;

        this.selectedAnswer = null;
        this.answered = false;
      }
    },
  },
};
</script>

<style scoped>
button {
  margin-top: 10px;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  margin-bottom: 10px;
}
</style>

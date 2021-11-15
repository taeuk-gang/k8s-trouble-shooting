<template>
  <div class="main-view">
    <header class="flex items-end justify-center pt-10">
      <div class="text-center">
        <h1
          class="text-yellow-300 text-white text-2xl sm:text-6xl text-center mb-2"
        >
          <router-link to="/">쿠버네티스 트러블 슈팅</router-link>
        </h1>
        <div
          v-if="pageCLI"
          class="mx-5 bg-black shadow-2xl rounded-lg overflow-hidden w-full"
        >
          <div id="header-buttons" class="py-3 px-4 flex">
            <div class="rounded-full w-3 h-3 bg-red-500 mr-2"></div>
            <div class="rounded-full w-3 h-3 bg-yellow-500 mr-2"></div>
            <div class="rounded-full w-3 h-3 bg-green-500"></div>
          </div>
          <div id="code-area" class="py-4 px-4 mt-1 text-white text-xl">
            <div class="mb-2">
              {{ pageCLI }}
            </div>
          </div>
        </div>
        <p class="text-base sm:text-2xl text-gray-500 mb-10 mt-5">
          {{ pageTitle }}
        </p>
      </div>
    </header>
    <main class="flex justify-center items-start">
      <Questions
        v-if="currentQuestions && !currentAnswers"
        :questions="currentQuestions"
      ></Questions>
      <Answer v-if="currentAnswers" :answers="currentAnswers"></Answer>
    </main>
  </div>
</template>

<script>
import Answer from "./Answer.vue";
import Questions from "./Questions.vue";

import QUESTIONS from "./../data/questions.json";

export default {
  name: "MainView",

  components: {
    Answer,
    Questions,
  },

  watch: {
    $route(to, from) {
      // eslint-disable-next-line no-console
      console.log("watch", to, from);
      this.changeRoute();
    },
  },

  data() {
    return {
      currentAnswers: undefined,
      currentQuestions: undefined,
      currentQuestion: undefined,
    };
  },

  computed: {
    pageTitle() {
      return this.currentQuestion !== undefined
        ? this.currentQuestion.title
        : "";
    },
    pageCLI() {
      return this?.currentQuestion?.cli ?? ``;
    },
  },

  mounted() {
    this.changeRoute();
  },

  methods: {
    changeRoute() {
      const path = this.$route.path;

      // eslint-disable-next-line no-console
      console.log(path, this.$route.path);

      if (path === "/") {
        this.currentAnswers = undefined;
        this.currentQuestions = QUESTIONS;
        this.currentQuestion = undefined;
      } else {
        const paths = path.split("/");

        let oldQuestion = { questions: QUESTIONS };
        let newQuestion;
        for (let i = 1; i < paths.length; ++i) {
          // eslint-disable-next-line no-console
          console.log("for", oldQuestion, paths[i]);
          newQuestion = this.getQuestion(paths[i], oldQuestion.questions);
          // eslint-disable-next-line no-console
          console.log("for 2", newQuestion, paths[i]);
          oldQuestion = newQuestion;
        }

        const question = oldQuestion;

        // eslint-disable-next-line no-console
        console.log({ question });
        if (question.questions !== undefined) {
          this.currentQuestions = question.questions;
          this.currentAnswers = undefined;
        } else if (question.answers !== undefined) {
          this.currentQuestions = undefined;
          this.currentAnswers = question.answers;
        }

        this.currentQuestion = question;
      }
    },

    getQuestion(path, questions) {
      return questions.find((question) => question.path === path);
    },
  },
};
</script>

<style>
.main-view {
  display: grid;
  width: 100%;
  grid-template-rows: 1fr minmax(min-content, 55vh);
  grid-row-gap: 0;
}
</style>

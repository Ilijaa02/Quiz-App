<template>
  <div class="container">
    <div id="app">
      <div class="quiz-container">
        <div class="quiz-header">
          <h1>Quiz App</h1>
        </div>
        <div class="step-progress" :style="{ width: progress + '%' }"></div>
        <div
          class="quiz-main"
          v-for="(element, index) in questions.slice(a, b)"
          :key="index"
          v-show="quiz"
        >
          <div class="box-question">
            <h2>Question {{ b }}/{{ questions.length }}</h2>
            <p class="question">{{ element.question }}</p>
          </div>
          <div class="box-suggestions">
            <ul>
              <li
                v-for="(item, index) in element.suggestions"
                :key="index"
                :class="select ? check(item) : ''"
                @click="selectResponse(item)"
              >
                {{ item.suggestion }}
                <fa
                  class="true"
                  icon="check"
                  v-if="select ? item.correct : ''"
                />
                <fa
                  class="false"
                  icon="times"
                  v-if="select ? !item.correct : ''"
                />
              </li>
            </ul>
          </div>
        </div>
        <div class="box-score" v-if="score_show">
          <h2>Your score is</h2>
          <h2>{{ score }}/{{ questions.length }}</h2>
          <div class="btn-restart">
            <button @click="restartQuiz">Restart <fa icon="sync" /></button>
          </div>
        </div>
        <div class="quiz-footer">
          <div class="box-button" v-if="progress < 100">
            <button
              @click="skipQuestion"
              :style="!next ? 'background-color:rgb(106,128,202)' : ''"
            >
              Skip
            </button>
            <button
              @click="nextQuestion"
              :style="next ? 'background-color:rgb(106,128,202)' : ''"
            >
              Next
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      questions: [
        {
          question: "Inside which HTMl element do we put the javascript ?",
          suggestions: [
            { suggestion: "<script>", correct: true },
            { suggestion: "<js>" },
            { suggestion: "<javascript>" },
            { suggestion: "<scriptjs>" },
          ],
        },
        {
          question:
            "What is the correct syntax for referring to an external script called 'xxx.js' ?",
          suggestions: [
            { suggestion: "<script href='xxx.js'>" },
            { suggestion: "<script name='xxx.js'>" },
            { suggestion: "<script src='xxx.js'>", correct: true },
            { suggestion: "<script id='xxx.js'>" },
          ],
        },
        {
          question: "How do you write 'Hello World' in an alert box ?",
          suggestions: [
            { suggestion: "msg('Hello World')" },
            { suggestion: "alertBox('Hello World')" },
            { suggestion: "alert('Hello World')", correct: true },
            { suggestion: "msgBox('Hello World')" },
          ],
        },
        {
          question: "How to write an IF statement in JavaScript ?",
          suggestions: [
            { suggestion: "if i = 5 then" },
            { suggestion: "if (i == 5)", correct: true },
            { suggestion: "if i == 5 then" },
            { suggestion: "if i = 5" },
          ],
        },
        {
          question: "How does a FOR loop start ?",
          suggestions: [
            { suggestion: "for i = 1 to 5" },
            { suggestion: "for (i <= 5; i++)" },
            { suggestion: "for (i = 0; i <= 5)" },
            { suggestion: "for (i = 0; i <= 5; i++)", correct: true },
          ],
        },
        {
          question: "How can you add a comment in a JavaScript ?",
          suggestions: [
            { suggestion: "'This is a comment'" },
            { suggestion: "//This is a comment", correct: true },
            { suggestion: "<!--This is a comment-->" },
            { suggestion: "*This is a comment" },
          ],
        },
      ],
      a: 0,
      b: 1,
      select: false,
      score: 0,
      quiz: true,
      score_show: false,
      next: false,
      progress: 0,
    };
  },
  methods: {
    selectResponse(e) {
      this.select = true;
      this.next = true;
      if (e.correct) {
        this.score++;
      }
    },
    check(status) {
      if (status.correct) {
        return "correct";
      } else {
        return "incorrect";
      }
    },
    nextQuestion() {
      if (!this.next) {
        return;
      }
      this.progress = this.progress + 100 / this.questions.length;
      if (this.questions.length - 1 == this.a) {
        this.score_show = true;
        this.quiz = false;
      } else {
        this.a++;
        this.b++;
        this.select = false;
        this.next = false;
      }
    },
    skipQuestion() {
      if (this.next) {
        return;
      }
      this.progress = this.progress + 100 / this.questions.length;
      if (this.questions.length - 1 == this.a) {
        this.score_show = true;
        this.quiz = false;
      } else {
        this.a++;
        this.b++;
      }
    },
    restartQuiz() {
      Object.assign(this.$data, this.$options.data());
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Inconsolata:wght@500&display=swap");
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#app {
  font-family: "Inconsolata", monospace;
  letter-spacing: 2px;
  display: flex;
  width: 100%;
  height: 100%;
  justify-content: center;
}
.quiz-container {
  display: flex;
  width: 60%;
  max-width: 700px;
  height: 85%;
  position: absolute;
  top: 0;
  bottom: 0;
  margin: auto;
  flex-direction: column;
  text-align: center;
  border: 1px solid #e7eae0;
  border-radius: 10px;
  background-color: white;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.19), 0 6px 6px rgba(0, 0, 0, 0.23);
}
.quiz-header {
  display: flex;
  width: 100%;
  height: 20%;
  border-bottom: 1px solid #e7eae0;
  justify-content: center;
  align-items: center;
  background-color: #e7eae0;
  border-radius: 10px 10px 0 0;
}
.quiz-main {
  display: flex;
  width: 100%;
  height: 70%;
  flex-direction: column;
  margin: auto;
}
.quiz-footer {
  display: flex;
  widows: 100%;
  height: 10%;
  justify-content: center;
  border-top: 1px solid #e7eae0;
  background-color: #e7eae0;
  border-radius: 0 0 10px 10px;
}
.question {
  margin-top: 15px;
}
.box-question {
  margin-top: 20px;
}
.box-suggestions {
  display: flex;
  width: 80%;
  justify-content: center;
  margin: auto;
}
ul {
  display: flex;
  width: 80%;
  margin: 0;
  padding: 0;
  flex-direction: column;
}
ul li {
  list-style: none;
  line-height: 2;
  margin-bottom: 20px;
  border-radius: 15px;
  border: 1px solid #e7eae0;
  cursor: pointer;
}
.box-button {
  display: flex;
  width: 100%;
}
.box-button button {
  width: 150px;
  height: 35px;
  border: none;
  color: white;
  font-size: 20px;
  cursor: pointer;
  border-radius: 15px;
  margin: auto;
  background-color: rgb(97, 161, 182);
}
.correct {
  border: 1px solid rgb(74, 219, 74);
  background-color: rgb(74, 219, 74);
  color: white;
  font-weight: 600;
}
.incorrect {
  border: 1px solid rgb(240, 117, 100);
  background-color: rgb(240, 117, 100);
  color: white;
  font-weight: 600;
}
.box-score {
  display: flex;
  width: 100%;
  height: 70%;
  flex-direction: column;
}
.box-score h2 {
  margin-top: 40px;
}
.btn-restart {
  display: flex;
  width: 100%;
  height: auto;
  justify-content: center;
  margin-top: 50px;
}
.btn-restart button {
  width: 150px;
  height: 35px;
  border: none;
  color: white;
  font-size: 20px;
  cursor: pointer;
  border-radius: 15px;
  margin: auto;
  background-color: rgb(106, 128, 202);
}
.true,
.false {
  float: right;
  margin-top: 8px;
  margin-right: 10px;
}
.step-progress {
  display: flex;
  width: 100%;
  height: 5px;
  background-color: rgb(106, 128, 202);
  transition: 0.5s;
}
@media screen and (max-width: 720px) {
  .quiz-container {
    width: 80%;
  }
  .box-button button {
    width: 100px;
  }
}
@media screen and (min-width: 720px) {
  .quiz-container {
    width: 80%;
  }
  li:hover {
    background-color: #e7eae0;
  }
}
</style>

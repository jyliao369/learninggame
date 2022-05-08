<template>
  <div class="textBoxCont">
    <div class="textBoxBorder">
      <div class="textBox">
        <!-- SYLLABLE COUNT FOR ATTACK -->
        <div class="questionCont" v-if="showSyllableQ">
          <div>
            <h2>{{ prompt }}</h2>
          </div>
          <div class="choiceBank">
            <button @click="checkSyllable(1)">1</button>
            <button @click="checkSyllable(2)">2</button>
            <button @click="checkSyllable(3)">3</button>
          </div>
        </div>

        <!-- PARTSOFSPEECH FOR SKILLS -->
        <div class="questionCont" v-if="showPartOfSpeechQ">
          <div>
            <h2>{{ prompt }}</h2>
          </div>
          <div class="wordBank">
            <button
              v-for="(word, index) in this.wordBank"
              :key="index"
              @click="checkAnswer(word)"
            >
              {{ word }}
            </button>
          </div>
        </div>

        <!-- ADDITION/SUBTRACTION COUNT FOR MAGIC -->
        <div class="questionCont" v-if="showMathQ">
          <div class="mathAnswer">
            <h2>{{ prompt }}</h2>
            <input input v-model="answer" type="number" placeholder="Answer" />
            <button @click="checkMathQuestion">Check</button>
          </div>
        </div>

        <!-- THIS IS THE ACTION BAR -->
        <div class="actionBar">
          <button @click="attack">Attack</button>
          <button @click="skill">Skills</button>
          <button @click="magic">Magic</button>
          <button @click="flee">Flee</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: [],
  data() {
    return {
      showSyllableQ: false,
      showPartOfSpeechQ: false,
      showMathQ: false,

      prompt: "",
    };
  },
  components: {},
  methods: {
    attack() {
      let randomWords = require("random-words");
      let word = randomWords();
      this.prompt = `How many syllables does ${word.toUpperCase()} have?`;

      this.showSyllableQ = !this.showSyllableQ;
      this.showPartOfSpeechQ = false;
      this.showMathQ = false;
    },
    skill() {
      this.showPartOfSpeechQ = !this.showPartOfSpeechQ;
      this.showSyllableQ = false;
      this.showMathQ = false;
    },
    magic() {
      let topic = Math.floor(Math.random() * 4);
      let valueA = Math.floor(Math.random() * 9);
      let valueB = Math.floor(Math.random() * 9);

      if (topic === 0) {
        this.prompt = valueA + " + " + valueB + " = ";
      } else if (topic === 1) {
        this.prompt = valueA + " - " + valueB + " = ";
      } else if (topic === 2) {
        this.prompt = valueA + " * " + valueB + " = ";
      } else if (topic === 3) {
        this.prompt = valueA + " / " + valueB + " = ";
      }

      this.showMathQ = !this.showMathQ;
      this.showSyllableQ = false;
      this.showPartOfSpeechQ = false;
    },
  },
  created() {
    // let randomWords = require("random-words");
    // let word = randomWords();
    // this.prompt = `How many syllables does ${word.toUpperCase()} have?`;
  },
};
</script>

<style scoped>
.textBoxCont {
  display: flex;
  justify-content: center;
  align-items: flex-end;
  height: 100vh;
  width: 100%;
  position: absolute;
}
.textBoxBorder {
  background: white;
  border-style: solid;
  border-radius: 10px;
}
.textBox {
  display: flex;
  flex-direction: row;
  justify-content: flex-end;
  width: 900px;
  border-style: solid;
  background: black;
  margin: 7px;
}
.actionBar {
  width: 25%;
}
.actionBar button {
  width: 225px;
  font-size: 25px;
  padding: 5px;
  padding-left: 15px;
  padding-right: 15px;
  cursor: pointer;
}

.questionCont {
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  align-items: center;
  width: 100%;
  padding: 10px;
  color: white;
}
.choiceBank {
  display: flex;
}
.choiceBank button {
  width: 150px;
  padding: 5px;
  margin: 10px;
}
.mathAnswer {
  display: flex;
  flex-direction: row;
}
.wordBank {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: space-evenly;
}
.wordBank button {
  width: 200px;
  font-size: 20px;
  padding: 5px;
  margin: 10px;
}
</style>

<template>
  <div>
    <button @click="partOfSpeech">Parts of Speech</button>
    <button @click="syllableQuestion">Syllable Count</button>
    <button @click="reset">Close/Reset</button>
  </div>

  <!-- THIS IS PARTS OF SPEECH -->
  <div class="questionCont" v-if="partOfSpeechQ">
    <div>
      <h1>{{ prompt }}</h1>
    </div>
    <div class="wordBank">
      <button v-for="(word, index) in this.wordBank" :key="index">
        {{ word }}
      </button>
      <button @click="checkAnswer('None of the Above')">
        None of the Above
      </button>
    </div>
  </div>

  <!-- THIS IS FOR SYLLABLES -->
  <div class="questionCont" v-if="syllableQ">
    <div>
      <h1>{{ prompt }}</h1>
    </div>
    <div class="choiceBank">
      <button @click="checkSyllable(1)">1</button>
      <button @click="checkSyllable(2)">2</button>
      <button @click="checkSyllable(3)">3</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      partOfSpeechQ: false,
      syllableQ: false,

      prompt: "",
      target: "",

      partsOfSpeech: [],
      wordBank: [],
    };
  },
  components: {},
  methods: {
    reset() {
      this.partOfSpeechQ = false;
      this.syllableQ = false;

      this.prompt = "";
      this.target = "";

      this.partsOfSpeech = [];
      this.wordBank = [];
    },
    partOfSpeech() {
      let randomWords = require("random-words");
      let pOfSpeech = ["noun", "verb", "adjective", "adverb"];

      this.partOfSpeechQ = true;
      this.target = pOfSpeech[Math.floor(Math.random() * pOfSpeech.length)];
      this.prompt = `Which of these words is/can be a(n) ${this.target}?`;

      this.wordBank = randomWords(4);
    },
    checkAnswer(word) {
      if (word !== "None of the Above") {
        fetch(`https://api.dictionaryapi.dev/api/v2/entries/en/${word}`)
          .then((response) => response.json())
          .then((data) => {
            for (let a = 0; a < data[0].meanings.length; a++) {
              if (data[0].meanings[a].partOfSpeech === this.target) {
                console.log("hit");
                this.score += 100;
                break;
              }
            }
          });
      } else {
        for (let a = 0; a < this.wordBank.length; a++) {
          fetch(
            `https://api.dictionaryapi.dev/api/v2/entries/en/${this.wordBank[a]}`
          )
            .then((response) => response.json())
            .then((data) => {
              for (let a = 0; a < data[0].meanings.length; a++) {
                if (data[0].meanings[a].partOfSpeech === this.target) {
                  console.log(
                    `One of these words can be/is a(n) ${this.target}`
                  );
                  break;
                }
              }
            });
        }
        console.log("Correct none of the above");
      }
    },
    syllableQuestion() {
      this.syllableQ = !this.syllableQ;
      let randomWords = require("random-words");
      let word = randomWords();
      this.prompt = `How many syllables does ${word.toUpperCase()} have?`;

      fetch(`https://wordsapiv1.p.rapidapi.com/words/${word}`, {
        method: "GET",
        headers: {
          "x-rapidapi-key": `${process.env.VUE_APP_API_KEY}`,
          "x-rapidapi-host": "wordsapiv1.p.rapidapi.com",
        },
      })
        .then((response) => response.json())
        .then((data) => (this.target = data.syllables.count));
    },
    checkSyllable(number) {
      console.log(this.target === number);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.questionCont {
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  align-items: center;
  width: 30%;
  height: 260px;
  border-style: solid;
  padding: 20px;
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
.choiceBank {
  display: flex;
}
.choiceBank button {
  width: 150px;
  padding: 5px;
  margin: 10px;
}
</style>

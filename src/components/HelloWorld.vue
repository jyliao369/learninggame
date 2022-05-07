<template>
  <div class="overCont">
    <!-- THIS HERO STATUS -->
    <div class="class">
      <button @click="startBattle">battle</button>
    </div>

    <div class="heroInfoCont">
      <div class="heroInfo">
        <h2>{{ hero.name }}</h2>
        <h2>HP: {{ hero.hp }}</h2>
      </div>
    </div>

    <!-- THIS BADGUY STATUS -->
    <div class="enemyInfoCont">
      <div class="enemyInfo" v-for="(enemy, index) in enemyTeam" :key="index">
        <h2>{{ enemy.name }}</h2>
        <h2>HP: {{ enemy.hp }}</h2>
      </div>
    </div>

    <!-- THIS IS THE PROMPT/TEXTBOX -->
    <div class="textBoxCont" v-if="textBoxCont">
      <div class="textBox">
        <!-- SYLLABLE COUNT FOR ATTACK -->
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

        <!-- ADDITION/SUBTRACTION COUNT FOR MAGIC -->
        <div class="questionCont" v-if="MathQ">
          <div class="mathAnswer">
            <h2>{{ prompt }}</h2>
            <input input v-model="answer" type="number" placeholder="Answer" />
            <button @click="checkMathQuestion">Check</button>
          </div>
        </div>

        <!-- PARTSOFSPEECH FOR SKILLS -->
        <div class="questionCont" v-if="partOfSpeechQ">
          <div>
            <h1>{{ prompt }}</h1>
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

        <!-- THIS IS THE ACTION BAR -->
        <div class="actionBar">
          <button @click="syllableQuestion">Attack</button>
          <button @click="partOfSpeech">Skills</button>
          <button @click="mathQuestion">Magic</button>
          <button @click="reset">Flee</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      partOfSpeechQ: false,
      syllableQ: false,
      MathQ: false,

      textBoxCont: false,
      prompt: "",
      target: "",
      answer: "",

      partsOfSpeech: [],
      wordBank: [],

      enemies: [
        { name: "slime", hp: 10 },
        { name: "mushroom", hp: 10 },
        { name: "red slime", hp: 15 },
        { name: "bat", hp: 15 },
        { name: "white rabbit", hp: 15 },
        { name: "wolf", hp: 20 },
        { name: "vulture", hp: 20 },
      ],
      hero: {
        name: "hero",
        hp: 20,
      },
      enemyTeam: [],
    };
  },
  components: {},
  methods: {
    reset() {
      this.partOfSpeechQ = false;
      this.syllableQ = false;
      this.additionQ = false;

      this.prompt = "";
      this.target = "";
      this.answer = "";

      this.partsOfSpeech = [];
      this.wordBank = [];
    },
    partOfSpeech() {
      this.partOfSpeechQ = !this.partOfSpeechQ;
      let randomWords = require("random-words");
      let pOfSpeech = ["noun", "verb", "adjective", "adverb"];
      this.target = pOfSpeech[Math.floor(Math.random() * pOfSpeech.length)];
      this.prompt = `Which of these words is/can be a(n) ${this.target}?`;

      this.wordBank = randomWords(4);
    },
    checkAnswer(word) {
      fetch(`https://api.dictionaryapi.dev/api/v2/entries/en/${word}`)
        .then((response) => response.json())
        .then((data) =>
          // (data) => console.log(data[0].meanings)
          {
            for (let a = 0; a < data[0].meanings.length; a++) {
              if (data[0].meanings[a].partOfSpeech === this.target) {
                this.reset();
                this.battle("Correct");
                break;
              }
              if (a === data[0].meanings.length - 1) {
                this.reset();
                this.battle("Incorrect");
                break;
              }
            }
          }
        );
    },
    syllableQuestion() {
      this.syllableQ = !this.syllableQ;
      let randomWords = require("random-words");
      let word = randomWords();
      this.prompt = `How many syllables does ${word.toUpperCase()} have?`;

      fetch(`https://wordsapiv1.p.rapidapi.com/words/${word}`, {
        method: "GET",
        headers: {
          "x-rapidapi-key": `dcfd1dec35msh39127802e6a5510p136b97jsnd9802a525c00`,
          "x-rapidapi-host": "wordsapiv1.p.rapidapi.com",
        },
      })
        .then((response) => response.json())
        // .then((data) => console.log(data));
        .then((data) => (this.target = data.syllables.count));
    },
    checkSyllable(number) {
      if (this.target === number) {
        this.answer = "Correct";
        this.reset();
        this.battle("Correct");
      } else {
        this.answer = "Incorrect";
        this.reset();
        this.battle("Incorrect");
      }
    },
    mathQuestion() {
      this.MathQ = !this.MathQ;
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
    },
    checkMathQuestion() {
      console.log("checking");
      let mathAnswer = eval(
        this.prompt
          .split(" ")
          .slice(0, this.prompt.split(" ").length - 2)
          .join("")
      );
      if (this.answer === mathAnswer) {
        this.MathQ = !this.MathQ;
        this.battle("Correct");
      } else {
        this.MathQ = !this.MathQ;
        this.battle("Incorrect");
      }
    },
    battle(answer) {
      if (answer === "Correct") {
        console.log(
          `${this.hero.name} does <5 damage> damage to ${this.enemies[0].name}`
        );
        this.enemyTeam[0].hp -= 5;
      } else {
        console.log(
          `${this.enemyTeam[0].name} does <5 damage> damage to ${this.hero.name}`
        );
        this.hero.hp -= 5;
      }
      this.checkIfDead();
    },
    checkIfDead() {
      if (this.enemyTeam[0].hp <= 0) {
        this.enemyTeam.pop();
        if (this.enemyTeam.length === 0) {
          this.textBoxCont = false;
        }
      }
    },
    startBattle() {
      let team = JSON.parse(
        JSON.stringify(
          this.enemies[Math.floor(Math.random() * this.enemies.length)]
        )
      );
      this.enemyTeam = [team];

      this.textBoxCont = true;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.class {
  position: absolute;
  z-index: 9;
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

.heroInfoCont {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  height: 100vh;
  width: 100%;
  position: absolute;
}
.enemyInfoCont {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  width: 100%;
  position: absolute;
}
.heroInfo,
.enemyInfo {
  display: flex;
  flex-direction: column;
  align-items: center;
  border-style: solid;
  padding: 25px;
}
.textBoxCont {
  display: flex;
  justify-content: center;
  align-items: flex-end;
  height: 100vh;
  width: 100%;
  position: absolute;
}
.textBox {
  display: flex;
  flex-direction: row;
  justify-content: flex-end;
  width: 900px;
  border-style: solid;
}
.questionCont {
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  align-items: center;
  width: 100%;
  padding: 20px;
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
</style>

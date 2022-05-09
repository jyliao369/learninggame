<template>
  <div class="heroInfoCont">
    <div>
      <div class="heroInfo">
        <h2>{{ hero.name }}</h2>
        <h2>HP: {{ hero.hp }}</h2>
        <h2>MP: {{ hero.mp }}</h2>
      </div>
    </div>
  </div>

  <div class="enemyInfoCont">
    <div
      class="enemyInfoBorder"
      v-for="(enemy, index) in enemyTeam"
      :key="index"
    >
      <div class="enemyInfo">
        <h2>{{ enemy.name }}</h2>
        <h2>HP: {{ enemy.hp }}</h2>
      </div>
    </div>
  </div>

  <div class="textBoxCont">
    <div class="textBoxBorder">
      <div class="textBox">
        <!-- SYLLABLE COUNT FOR ATTACK -->
        <div class="questionCont" v-if="showSyllableQ">
          <div>
            <h2>{{ prompt }}</h2>
          </div>
          <div class="choiceBank">
            <button @click="checkAttack(1)">1</button>
            <button @click="checkAttack(2)">2</button>
            <button @click="checkAttack(3)">3</button>
            <button @click="checkAttack(4)">4</button>
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
          <button @click="giveCoin">test</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["heroInfo"],
  data() {
    return {
      showSyllableQ: false,
      showPartOfSpeechQ: false,
      showMathQ: false,

      prompt: "",
      wordBank: [],
      target: "",

      enemyTeam: [],
      enemies: [
        { name: "slime", hp: 10 },
        { name: "mushroom", hp: 10 },
        { name: "red slime", hp: 15 },
        { name: "bat", hp: 15 },
        { name: "white rabbit", hp: 15 },
        { name: "wolf", hp: 20 },
        { name: "vulture", hp: 20 },
      ],

      hero: this.heroInfo,
    };
  },
  components: {},
  methods: {
    attack() {
      let randomWords = require("random-words");
      let word = randomWords();
      this.prompt = `How many syllables does ${word.toUpperCase()} have?`;
      fetch(`https://wordsapiv1.p.rapidapi.com/words/${word}`, {
        method: "GET",
        headers: {
          "x-rapidapi-key": `${process.env.VUE_APP_WORDAPI_KEY}`,
          "x-rapidapi-host": "wordsapiv1.p.rapidapi.com",
        },
      })
        .then((response) => response.json())
        // .then((data) => console.log(data));
        .then((data) => (this.target = data.syllables.count));

      this.showSyllableQ = !this.showSyllableQ;
      this.showPartOfSpeechQ = false;
      this.showMathQ = false;
    },
    checkAttack(number) {
      if (this.target === number) {
        console.log("Correct");
        this.showSyllableQ = !this.showSyllableQ;
        this.damage("Correct");
      } else {
        console.log("Incorrect");
        this.showSyllableQ = !this.showSyllableQ;
        this.damage("Incorrect");
      }
    },
    skill() {
      let randomWords = require("random-words");
      this.wordBank = randomWords(4);
      let pOfSpeech = ["noun", "verb", "adjective", "adverb"];
      this.prompt = `Which of these words is/can be a(n) ${
        pOfSpeech[Math.floor(Math.random() * pOfSpeech.length)]
      }?`;

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

    flee() {
      this.$emit("flee");
    },
    damage(answer) {
      if (answer === "Correct") {
        this.enemyTeam[0].hp -= 5;
        this.checkEnemy();
      } else if (answer === "Incorrect") {
        this.hero.hp -= 5;
        this.checkPlayer();
      }
    },
    checkEnemy() {
      console.log(this.enemyTeam[0].hp);
      if (this.enemyTeam[0].hp <= 0) {
        this.enemyTeam.pop();
      }

      if (this.enemyTeam.length <= 0) {
        this.giveCoin();
      }
    },
    checkPlayer() {
      if (this.hero.hp <= 0) {
        console.log("GAME OVER");
      }
    },
    giveCoin() {
      this.hero.gold += 5;
    },
  },
  created() {
    // let randomWords = require("random-words");
    // let word = randomWords();
    // this.prompt = `How many syllables does ${word.toUpperCase()} have?`;
    // this.enemyTeam = [
    //   this.enemies[Math.floor(Math.random() * this.enemies.length)],
    // ];
    let team = JSON.parse(
      JSON.stringify(
        this.enemies[Math.floor(Math.random() * this.enemies.length)]
      )
    );
    this.enemyTeam = [team];
  },
};
</script>

<style scoped>
.heroInfoCont {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  height: 100vh;
  width: 100%;
  position: absolute;
}
.heroInfo {
  display: flex;
  flex-direction: column;
  align-items: center;
  border-style: solid;
  padding: 25px;
  background: black;
  color: white;
}
.enemyInfoCont {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  width: 100%;
  position: absolute;
}
.enemyInfoBorder {
  background: white;
}
.enemyInfo {
  display: flex;
  flex-direction: column;
  align-items: center;
  border-style: solid;
  padding: 25px;
  background: black;
  color: white;
}

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
  width: 120px;
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

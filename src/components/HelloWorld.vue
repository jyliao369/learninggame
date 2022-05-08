<template>
  <div class="overCont">
    <!-- THIS HERO STATUS -->
    <div class="class">
      <h1>{{ test }}</h1>
      <button @click="startBattle">battle</button>
      <button @click="showBag">inventory</button>
      <button @click="showStats">stats</button>
      <button @click="showMain">back</button>
    </div>

    <div class="heroInfoCont" v-if="showStatistics">
      <div>
        <div class="heroInfo">
          <h2>{{ hero.name }}</h2>
          <h2>HP: {{ hero.hp }}</h2>
        </div>
      </div>
    </div>

    <!-- THIS IS THE WINDOWS FOR TOWNS AND STUFF -->
    <div class="mainWindowCont" v-if="showMainWindow">
      <div class="mainWindowBorder">
        <div class="mainWindow">
          <h1>Main Window</h1>
        </div>
      </div>
    </div>

    <!-- THIS BADGUY STATUS -->
    <div class="enemyInfoCont" v-if="showEnemyStats">
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

    <div class="inventoryCont" v-if="showInventory">
      <div class="inventoryBorder">
        <div class="inventory">
          <h2>Inventory</h2>
        </div>
      </div>
    </div>

    <!-- THIS IS THE PROMPT/TEXTBOX -->
    <div class="textBoxCont" v-if="textBoxCont">
      <div class="textBoxBorder">
        <div class="textBox">
          <!-- SYLLABLE COUNT FOR ATTACK -->
          <div class="questionCont" v-if="syllableQ">
            <div>
              <h2>{{ prompt }}</h2>
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
              <input
                input
                v-model="answer"
                type="number"
                placeholder="Answer"
              />
              <button @click="checkMathQuestion">Check</button>
            </div>
          </div>

          <!-- PARTSOFSPEECH FOR SKILLS -->
          <div class="questionCont" v-if="partOfSpeechQ">
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

          <!-- THIS IS THE ACTION BAR -->
          <div class="actionBar">
            <button @click="syllableQuestion">Attack</button>
            <button @click="partOfSpeech">Skills</button>
            <button @click="mathQuestion">Magic</button>
            <button @click="flee">Flee</button>
          </div>
        </div>
      </div>
    </div>

    <!-- CITY MARKERS -->
    <div class="cityMapCont" v-if="showMap">
      <div class="cityMap">
        <div @click="showMain" class="city"></div>
        <div @click="showMain" class="city1"></div>
        <div @click="showMain" class="city2"></div>
        <div @click="showMain" class="city3"></div>
        <div @click="showMain" class="city4"></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      test: process.env.VUE_APP_WORDAPI_KEY,
      partOfSpeechQ: false,
      syllableQ: false,
      MathQ: false,

      textBoxCont: false,
      showInventory: false,
      showStatistics: false,
      showMainWindow: false,
      showEnemyStats: false,
      showMap: true,
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
    showMain() {
      this.showMainWindow = !this.showMainWindow;
      this.showMap = !this.showMap;
    },
    showStats() {
      this.showStatistics = !this.showStatistics;
    },
    showBag() {
      this.showInventory = !this.showInventory;
    },
    flee() {
      this.textBoxCont = !this.textBoxCont;
      this.showMap = !this.showMap;
      this.showStatistics = false;
      this.partOfSpeechQ = false;
      this.syllableQ = false;
      this.MathQ = false;
      this.enemyTeam = [];
    },
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
      this.syllableQ = false;
      this.MathQ = false;
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
      this.MathQ = false;
      this.partOfSpeechQ = false;
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
      this.partOfSpeechQ = false;
      this.syllableQ = false;
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
          this.showStatistics = false;
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

      this.showMap = !this.showMap;
      this.showEnemyStats = true;
      this.showStatistics = true;
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

.overCont {
  width: 100%;
  height: 100vh;
  background-image: url(../../image/map.png);
  background-repeat: no-repeat;
  background-size: cover;
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
.enemyInfoBorder {
  background: white;
}
.heroInfo,
.enemyInfo {
  display: flex;
  flex-direction: column;
  align-items: center;
  border-style: solid;
  padding: 25px;
  background: black;
  color: white;
}
.mainWindowCont {
  display: flex;
  height: 100vh;
  width: 100%;
  position: absolute;
  justify-content: center;
  align-items: center;
}
.mainWindowBorder {
  height: 80vh;
  width: 65%;
  border-style: solid;
  border-width: 2px;
  background: white;
}

.inventoryCont {
  display: flex;
  align-items: center;
  justify-content: flex-end;
  height: 100vh;
  width: 100%;
  position: absolute;
}
.inventoryBorder {
  background: white;
  border-style: solid;
  border-width: 2px;
  border-radius: 10px;
}
.inventory {
  display: flex;
  justify-content: center;
  height: 550px;
  width: 275px;
  border-style: solid;
  border-radius: 10px;
  background: black;
  margin: 5px;
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
.cityMapCont {
  height: 100vh;
  width: 100%;
  position: absolute;
}
.city {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  border-style: solid;
  border-width: thick;
  background: #80c8ff;
  position: absolute;
  margin-top: 290px;
  margin-left: 825px;
}
.city1 {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  border-style: solid;
  border-width: thick;
  background: #80c8ff;
  position: absolute;
  margin-top: 600px;
  margin-left: 550px;
}
.city2 {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  border-style: solid;
  border-width: thick;
  background: #80c8ff;
  position: absolute;
  margin-top: 520px;
  margin-left: 1590px;
}
.city3 {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  border-style: solid;
  border-width: thick;
  background: #80c8ff;
  position: absolute;
  margin-top: 630px;
  margin-left: 1185px;
}
.city4 {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  border-style: solid;
  border-width: thick;
  background: #80c8ff;
  position: absolute;
  margin-top: 370px;
  margin-left: 100px;
}
</style>

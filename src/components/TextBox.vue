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
        <!-- CHOICES FOR SKILLS / MAGIC -->
        <div class="skillChoices" v-if="showSkills">
          <button @click="skill">Skill Lvl. 1</button>
          <button @click="skillTwo">Skill Lvl. 2</button>
          <button>Skill Lvl. 3</button>
        </div>
        <div class="magicChoices" v-if="showMagic">
          <button @click="magic">Magic Lvl. 1</button>
          <button @click="magicTwo">Magic Lvl. 2</button>
          <button @click="magicThree">Magic Lvl. 3</button>
        </div>

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

        <!-- PART OF SPEECH SENTENCE -->
        <div class="questionCont" v-if="showSentenceQ">
          <div>
            <h2>{{ prompt }}</h2>
          </div>
          <div class="sentenceCont">
            <p
              v-for="(word, index) in this.mainSentence"
              :key="index"
              @click="sentenceCheck(word)"
            >
              {{ word }}
            </p>
            <p></p>
          </div>
        </div>

        <!-- ADDITION/SUBTRACTION COUNT FOR MAGIC -->
        <div class="questionCont" v-if="showMathQ">
          <div class="mathAnswer">
            <h2>{{ prompt }}</h2>
            <input v-model="answer" type="number" placeholder="Answer" />
            <button @click="checkMagic">Check</button>
          </div>
        </div>

        <!-- THIS IS THE ACTION BAR -->
        <div class="actionBar">
          <button @click="attack">Attack</button>
          <button @click="showAllSkills">Skills</button>
          <button @click="showAllMagic">Magic</button>
          <button @click="flee">Flee</button>
          <!-- <button @click="giveCoin">test</button> -->
        </div>
      </div>
    </div>
  </div>
</template>

<script>
const { sentence } = require("txtgen/dist/cjs/txtgen.js");

export default {
  props: ["heroInfo"],
  data() {
    return {
      showSyllableQ: false,
      showPartOfSpeechQ: false,
      showSentenceQ: false,
      showMathQ: false,
      showSkills: false,
      showMagic: false,

      prompt: "",
      wordBank: [],
      target: "",
      mainSentence: "",

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
    showAllSkills() {
      this.showSkills = !this.showSkills;
      this.showMagic = false;

      this.showSyllableQ = false;
      this.showPartOfSpeechQ = false;
      this.showSentenceQ = false;
      this.showMathQ = false;
    },
    skill() {
      let randomWords = require("random-words");
      this.wordBank = randomWords(4);
      let pOfSpeech = ["noun", "verb", "adjective", "adverb"];
      this.prompt = `Which of these words is/can be a(n) ${
        pOfSpeech[Math.floor(Math.random() * pOfSpeech.length)]
      }?`;

      this.showSkills = !this.showSkills;
      this.showPartOfSpeechQ = !this.showPartOfSpeechQ;
      this.showSyllableQ = false;
      this.showMathQ = false;
    },
    skillTwo() {
      let pOfSpeech = ["noun", "verb", "adjective", "adverb"];
      let targetPartofS =
        pOfSpeech[Math.floor(Math.random() * pOfSpeech.length)];

      this.target = targetPartofS;
      this.prompt = `Find a(n) ${targetPartofS} in this sentence?`;
      this.mainSentence = sentence().split(" ");

      this.showSkills = !this.showSkills;
      this.showSentenceQ = !this.showSentenceQ;
      this.showSyllableQ = false;
      this.showMathQ = false;
    },
    sentenceCheck(word) {
      let mainWord = word
        .split("")
        .filter(
          (char) =>
            char !== "," &&
            char !== "!" &&
            char !== "?" &&
            char !== "." &&
            char !== ";"
        )
        .join("");
      // console.log(mainWord);
      // console.log(this.target);

      fetch(
        `https://www.dictionaryapi.com/api/v3/references/collegiate/json/${mainWord}?key=${process.env.VUE_APP_DICTION_KEY}`
      )
        .then((response) => response.json())
        .then((data) => {
          // console.log(data);
          for (let a = 0; a < data.length; a++) {
            if (data[a].fl === this.target) {
              this.damage("Correct");
              this.showSentenceQ = false;
              break;
            }
            if (a === data.length - 1) {
              this.damage("Incorrect");
              this.showSentenceQ = false;
              break;
            }
          }
        });
    },
    // THESE ARE FOR ALL MAGIC/MATH
    showAllMagic() {
      this.showMagic = !this.showMagic;
      this.showSkills = false;

      this.showSyllableQ = false;
      this.showPartOfSpeechQ = false;
      this.showSentenceQ = false;
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

      this.showMagic = !this.showMagic;
      this.showMathQ = !this.showMathQ;
      this.showSyllableQ = false;
      this.showPartOfSpeechQ = false;
    },
    magicTwo() {
      let topic = Math.floor(Math.random() * 4);
      let valueA = (Math.random() * 0.9).toFixed(1);
      let valueB = (Math.random() * 0.9).toFixed(1);

      if (topic === 0) {
        this.prompt = valueA + " + " + valueB + " = ";
      } else if (topic === 1) {
        this.prompt = valueA + " - " + valueB + " = ";
      } else if (topic === 2) {
        this.prompt = valueA + " * " + valueB + " = ";
      } else if (topic === 3) {
        this.prompt = valueA + " / " + valueB + " = ";
      }

      this.showMagic = !this.showMagic;
      this.showMathQ = !this.showMathQ;
      this.showSyllableQ = false;
      this.showPartOfSpeechQ = false;
    },
    magicThree() {
      let topic = Math.floor(Math.random() * 4);
      let valueA = `1 / ${Math.floor(Math.random() * 9)}`;
      let valueB = `1 / ${Math.floor(Math.random() * 9)}`;

      if (topic === 0) {
        this.prompt = valueA + " + " + valueB + " = ";
      } else if (topic === 1) {
        this.prompt = valueA + " - " + valueB + " = ";
      } else if (topic === 2) {
        this.prompt = valueA + " * " + valueB + " = ";
      } else if (topic === 3) {
        this.prompt = valueA + " / " + valueB + " = ";
      }

      this.showMagic = !this.showMagic;
      this.showMathQ = !this.showMathQ;
      this.showSyllableQ = false;
      this.showPartOfSpeechQ = false;
    },
    magicFour() {},
    checkMagic() {
      let mathAnswer = eval(
        this.prompt
          .split(" ")
          .slice(0, this.prompt.split(" ").length - 2)
          .join("")
      );
      console.log(mathAnswer);
      if (this.answer === mathAnswer) {
        this.showMathQ = !this.showMathQ;
        this.damage("Correct");
      } else {
        this.showMathQ = !this.showMathQ;
        this.damage("Incorrect");
      }
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
.magicChoices,
.skillChoices {
  display: flex;
  height: 100%;
  width: 100%;
  color: white;
  flex-direction: column;
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
.sentenceCont {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
}
.sentenceCont p {
  margin-left: 5px;
  font-size: 18px;
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

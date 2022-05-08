<template>
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
    <button @click="damage">hello</button>
  </div>
</template>

<script>
export default {
  props: [],
  data() {
    return {
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
    };
  },
  components: {},
  methods: {
    // damage() {
    //   console.log("hello");
    //   console.log(this.enemyTeam[0].hp);
    //   this.enemyTeam[0].hp -= 5;
    // },
  },
  created() {
    // let team = JSON.parse(
    //   JSON.stringify(
    //     this.enemies[Math.floor(Math.random() * this.enemies.length)]
    //   )
    // );
    this.enemyTeam = [
      this.enemies[Math.floor(Math.random() * this.enemies.length)],
    ];
  },
};
</script>

<style scoped>
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
</style>

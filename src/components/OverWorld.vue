<template>
  <div class="overCont">
    <!-- NOTE THE FARTHER DOWN THE CLOSER TO USER -->

    <!-- THIS IS THE CITY MAP -->
    <CityMap v-if="showCityMap" @open="showMap" />

    <!-- MAIN WINDOWS FOR TOWNS AND LOCATIONS  -->
    <MainWindow v-if="showMainWindow" />

    <!-- INVENTORY -->
    <InvenBag v-if="showInventory" />

    <!-- HERO INFO -->
    <!-- <HeroInfo v-if="showHeroStats" v-bind:heroInfo="this.hero" /> -->

    <!-- ENEMY INFO -->
    <!-- <EnemyInfo v-if="showEnemyStats" /> -->

    <!-- TEXTBOX -->
    <TextBox v-if="showTextBox" v-bind:heroInfo="this.hero" />

    <div class="class">
      <button @click="showEnemyInfo">battle</button>
      <button @click="showInvenBag">inventory</button>
      <button @click="showHeroInfo">stats</button>
      <button @click="reset">back</button>
    </div>
  </div>
</template>

<script>
import InvenBag from "./InvenBag.vue";
import CityMap from "./CityMap.vue";
import MainWindow from "./MainWindow.vue";
// import HeroInfo from "./HeroInfo.vue";
// import EnemyInfo from "./EnemyInfo.vue";
import TextBox from "./TextBox.vue";

export default {
  data() {
    return {
      showInventory: false,
      showCityMap: true,
      showMainWindow: false,
      showHeroStats: false,
      showEnemyStats: false,
      showTextBox: false,

      hero: {
        name: "hero",
        hp: 20,
      },
    };
  },
  components: {
    InvenBag,
    CityMap,
    MainWindow,
    // HeroInfo,
    // EnemyInfo,
    TextBox,
  },
  methods: {
    reset() {
      this.showCityMap = true;
      this.showMainWindow = false;
    },
    showInvenBag() {
      this.showInventory = !this.showInventory;
    },
    showMap() {
      this.showCityMap = !this.showCityMap;
      this.showMainWindow = !this.showMainWindow;
    },
    showHeroInfo() {
      this.showHeroStats = !this.showHeroStats;
    },
    showEnemyInfo() {
      this.showTextBox = !this.showTextBox;
      this.showCityMap = !this.showCityMap;
    },
  },
};
</script>

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
</style>

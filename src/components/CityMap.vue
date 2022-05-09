<template>
  <div class="cityMapCont">
    <div class="cityMap">
      <button @click="showCity(citites[0])" class="city"></button>
      <button @click="showCity(citites[1])" class="city1"></button>
      <button @click="showCity(citites[2])" class="city2"></button>
      <button @click="showCity(citites[3])" class="city3"></button>
      <button @click="showCity(citites[4])" class="city4"></button>
    </div>
  </div>

  <MainWindow
    @leaveCity="showCity"
    v-if="openCity"
    v-bind:cityInfo="this.currentCity"
  />
</template>

<script>
import MainWindow from "../components/MainWindow.vue";

export default {
  data() {
    return {
      openCity: false,

      currentCity: {},
      citites: [
        {
          name: "TerraFirma",
          shopItem: ["potion", "antidote"],
          shopArmor: ["wooden shield", "wooden helmet"],
          shopWeapon: ["wooden dagger", "wooden sword"],
        },
        {
          name: "Kodon",
          shopItem: ["super potion", "super antidote"],
          shopArmor: ["bronze shield", "bronze helmet"],
          shopWeapon: ["bronze dagger", "bronze sword"],
        },
        {
          name: "Broria",
          shopItem: ["great potion", "great antidote"],
          shopArmor: ["iron shield", "iron helmet"],
          shopWeapon: ["iron dagger", "iron sword"],
        },
        {
          name: "Tarlens",
          shopItem: ["ultra potion", "ultra antidote"],
          shopArmor: ["silver shield", "silver helmet"],
          shopWeapon: ["silver dagger", "silver sword"],
        },
        {
          name: "Wrenchester",
          shopItem: ["mega potion", "mega antidote"],
          shopArmor: ["diamond shield", "diamond helmet"],
          shopWeapon: ["diamond dagger", "diamond sword"],
        },
      ],
    };
  },
  components: { MainWindow },
  methods: {
    showCity(city) {
      console.log(city);
      this.currentCity = city;
      this.openCity = !this.openCity;
    },
    // openCity() {
    //   this.$emit("open");
    // },
    // leaveCity() {
    //   this.showCity = true;
    // },
  },
};
</script>

<style scoped>
.cityMapCont {
  height: 100vh;
  width: 100%;
  position: absolute;
}
.city {
  width: 10px;
  height: 20px;
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
  height: 20px;
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
  height: 20px;
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
  height: 20px;
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
  height: 20px;
  border-radius: 50%;
  border-style: solid;
  border-width: thick;
  background: #80c8ff;
  position: absolute;
  margin-top: 370px;
  margin-left: 100px;
}
</style>

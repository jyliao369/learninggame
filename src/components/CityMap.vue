<template>
  <!-- v-if="showMap" -->
  <div class="cityMapCont">
    <div class="cityMap">
      <button @click="openCity" class="city"></button>
      <button @click="openCity" class="city1"></button>
      <button @click="openCity" class="city2"></button>
      <button @click="openCity" class="city3"></button>
      <button @click="openCity" class="city4"></button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {};
  },
  components: {},
  methods: {
    openCity() {
      this.$emit("close");
    },
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

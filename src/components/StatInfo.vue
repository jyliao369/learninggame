<template>
  <div class="heroInfoCont">
    <div>
      <div class="heroInfo">
        <h2>{{ heroInfo.name }}</h2>
        <h2>HP: {{ heroInfo.hp }}</h2>
        <h2>MP: {{ heroInfo.mp }}</h2>
        <h2>Gold: {{ heroInfo.gold }}</h2>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["heroInfo"],
  data() {
    return;
  },
  components: {},
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
</style>

<template>
  <div class="mainWindowCont">
    <div class="mainWindowBorder">
      <div class="mainWindow">
        <div class="citySign">
          <h1>{{ cityInfo.name }}</h1>
        </div>
        <div class="shopWindow">
          <div class="shopItem" v-if="itemStore">
            <div v-for="(item, index) in cityInfo.shopItem" :key="index">
              <p>{{ item }}</p>
            </div>
          </div>
          <div class="shopArmor" v-if="armorStore">
            <div v-for="(armor, index) in cityInfo.shopArmor" :key="index">
              <p>{{ armor }}</p>
            </div>
          </div>
          <div class="shopWeapon" v-if="weaponStore">
            <div v-for="(weapon, index) in cityInfo.shopWeapon" :key="index">
              <p>{{ weapon }}</p>
            </div>
          </div>
        </div>
        <div class="townChoice">
          <button>Palace</button>
          <button>Bar/Meeting Place</button>
          <button>Blacksmith</button>
          <button @click="openStore(`Item`)">Item Shop</button>
          <button @click="openStore(`Weapon`)">Weapon Shop</button>
          <button @click="openStore(`Armor`)">Armor Shop</button>
          <button @click="leaveCity">Leave Town/City</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["cityInfo"],
  data() {
    return {
      itemStore: false,
      armorStore: false,
      weaponStore: false,
    };
  },
  components: {},
  methods: {
    leaveCity() {
      this.$emit("leaveCity");
    },
    openStore(type) {
      if (type === "Item") {
        this.itemStore = !this.itemStore;
        this.armorStore = false;
        this.weaponStore = false;
      } else if (type === "Armor") {
        this.itemStore = false;
        this.armorStore = !this.armorStore;
        this.weaponStore = false;
      } else if (type === "Weapon") {
        this.itemStore = false;
        this.armorStore = false;
        this.weaponStore = !this.weaponStore;
      }
    },
  },
};
</script>

<style scoped>
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
.mainWindow {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
  height: 100%;
  position: relative;
}
.citySign {
  position: absolute;
}
.shopWindow {
  display: flex;
  align-items: center;
  height: 100%;
  width: 100%;
}
.shopItem,
.shopArmor,
.shopWeapon {
  display: flex;
  flex-direction: column;
  height: 250px;
  width: 225px;
  border-style: solid;
  position: absolute;
  padding: 10px;
}
.shopItem p,
.shopArmor p,
.shopWeapon p {
  font-size: 20px;
  padding: 7px;
}
.townChoice {
  display: flex;
  justify-content: space-between;
  width: 100%;
}
.townChoice button {
  width: 190px;
  padding: 5px;
  font-size: 10px;
}
</style>

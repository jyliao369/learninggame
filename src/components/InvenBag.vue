<template>
  <div class="inventoryCont">
    <div class="inventoryBorder">
      <div class="inventory">
        <h2>Inventory</h2>
      </div>
    </div>
  </div>
</template>

<script>
export default {};
</script>

<style scoped>
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
</style>

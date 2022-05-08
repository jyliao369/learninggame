<template>
  <div class="mainWindowCont">
    <div class="mainWindowBorder">
      <div class="mainWindow">
        <h1>Main Window</h1>
      </div>
    </div>
  </div>
</template>
<script>
export default {};
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
</style>

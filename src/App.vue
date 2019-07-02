<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
export default {};
</script>

<style lang="scss">
  @import "assets/main";

  #app {
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 50px 0;
  }
</style>

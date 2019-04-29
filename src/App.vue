<template lang="pug">
#app
  router-view
</template>

<style lang="sass">
@import "@/styles/global.sass"

// ====================
//      MAIN
// ==============================
#app
  width: 100%
  min-height: 100vh
  +center_flex(flex-start)
  flex-direction: column
</style>

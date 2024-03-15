<template>
  <div class="top" id="app">
    <SimonGame/>
  </div>
</template>

<script>
import SimonGame from './components/SimonGame.vue'

export default {
  name: 'App',
  components: {
    SimonGame
  }
}
</script>

<style lang="sass">
*
  padding: 0
  margin: 0

.top
  display: flex
  justify-content: center
  align-items: center

.center
  padding-left: calc(50% - 1200px / 2)
  padding-right: calc(50% - 1200px / 2)
</style>

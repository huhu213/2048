<template>
  <div id="app">
    <h1>This is huhu's 2048 demo!</h1>
    <grid></grid>
  </div>
</template>

<script>
import Grid from './components/grid'

export default {
  components: {
    Grid
  }
}
</script>

<style>
  h1 {
    text-align: center;
  }
  grid {
    margin-top: 20px;
  }
</style>

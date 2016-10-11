<template>
  <div class="layer-wrap">
    <div class="layer">
      <h1>Game Over!</h1>
    </div>
  </div>
</template>

<script>
export default {
  created () {
    var wrap = document.getElementsByClassNAme('layer-wrap')[0];
    wrap.style.width = document.pageX;
    wrap.style.height = document.pageY;
  }
}
</script>

<style scope>
  .layer-wrap {
    position: absolute;
    left: 0;
    top: 0;
    z-index: 999;
    background: rgba(0, 0, 0, 0.5);
  }
  .layer {
    width: 300px;
    height: 300px;
    background-color: rgba(238, 228, 218, 0.35);
    position: absolute;
    margin: auto;
    text-align: center;
    line-height: 300px;
  }
</style>

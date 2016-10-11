<template>
  <div class="gameGrid">
    <div class="cell" v-for="i in cells" v-bind="{id: i, 'data-value': values[i-1]}">{{values[i-1] === 0 ? '' : values[i-1]}}</div>
    <layer v-show="isGameOver"></layer>
  </div>
</template>

<script>
import Layer from './Layer'
var cellIds = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16];
var cellValue = [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0];

export default {
  data () {
    return {
      "cells": cellIds,
      "values": cellValue,
      "isGameOver": false
    }
  },
  methods: {
    isGameOver() {
      for (let i = 0; i < 16; ++ i) {
        if(cellValue[i] == 0) return false;
      }
      var dx = [0, 0, -1, 1];
      var dy = [-1, 1, 0, 0];
      for (let i = 0; i < 4; ++ i) {
        for (let j = 0; j < 4; ++ j) {
          for (let d = 0; d < 4; ++ d) {
            let nx = i + dx[d];
            let ny = j + dy[d];
            if (nx < 0 || nx >= 4 || ny < 0 || ny >= 4) continue;
            if (cellValue[i * 4 + j] == cellValue[nx * 4 + ny]) return false;
          }
        }
      }
      return true;
    },
    init () {
      var firstCell = Math.floor(Math.random()*16);
      var secondCell = Math.floor(Math.random()*16);
      while(secondCell == firstCell) {
        secondCell = Math.floor(Math.random()*16);
      }
      this.values[firstCell] = 2;
      this.values[secondCell] = 2;
    },
    moveLeft () {
      for (let i = 0; i < 4; ++ i) {
        var idx = 0;
          for (let j = 0; j < 4; ++ j) {
            if (cellValue[i * 4 + j]) {
                cellValue[i * 4 + idx] = cellValue[i * 4 + j];
                idx ++;
            }
          }
          for (let j = idx; j < 4; ++ j) {
            cellValue[i * 4 + j] = 0;
          }
      }
      for (let i = 0; i < 4; ++ i) {
        for (let j = 0; j < 3; ++ j) {
          if (cellValue[i * 4 + j] == cellValue[i * 4 + j + 1]) {
            cellValue[i * 4 + j + 1] = 0;
            cellValue[i * 4 + j] *= 2;
          }
        }
      }
      for (let i = 0; i < 4; ++ i) {
        let idx = 0;
        for (let j = 0; j < 4; ++ j) {
            if (cellValue[i * 4 + j]) {
                cellValue[i * 4 + idx] = cellValue[i * 4 + j];
                idx ++;
            }
        }
        for (let j = idx; j < 4; ++ j) {
          cellValue[i * 4 + j] = 0;
        }
      }
      for(var i = 0; i < 16; i ++) {
        this.$set(this.values, i, cellValue[i]);
      }
    },
    moveDown () {
      for (let j = 0; j < 4; ++ j) {
        var idx = 3;
          for (let i = 3; i >= 0; -- i) {
            if (cellValue[i * 4 + j]) {
                cellValue[idx * 4 + j] = cellValue[i * 4 + j];
                idx --;
            }
          }
          for (let i = 0; i <= idx; ++ i) {
            cellValue[i * 4 + j] = 0;
          }
      }
      for (let j = 0; j < 4; ++ j) {
        for (let i = 3; i > 0; -- i) {
          if (cellValue[i * 4 + j] == cellValue[i * 4 + j - 4]) {
            cellValue[i * 4 + j] *= 2;
            cellValue[i * 4 + j - 4] = 0;
          }
        }
      }
      for (let j = 0; j < 4; ++ j) {
        var idx = 3;
          for (let i = 3; i >= 0; -- i) {
            if (cellValue[i * 4 + j]) {
                cellValue[idx * 4 + j] = cellValue[i * 4 + j];
                idx --;
            }
          }
          for (let i = 0; i <= idx; ++ i) {
            cellValue[i * 4 + j] = 0;
          }
      }

      for(var i = 0; i < 16; i ++) {
        this.$set(this.values, i, cellValue[i]);
      }
    },
    moveUp () {
      for (let i = 0; i < 4; ++ i) {
        let idx = 0;
        for (let j = 0; j < 4; ++ j) {
          if (cellValue[j * 4 + i]) {
            cellValue[idx * 4 + i] = cellValue[j * 4 + i];
            idx ++;
          }
        }
        for (let j = idx; j < 4; ++ j) {
          cellValue[j * 4 + i] = 0;
        }
      }
      for (let i = 0; i < 4; ++ i) {
        for (let j = 0; j < 3; ++ j) {
          if (cellValue[j * 4 + i] == cellValue[j * 4 + 4 + i]) {
            cellValue[j * 4 + i] *= 2;
            cellValue[j * 4 + 4 + i] = 0;
          }
        }
      }
      for (let i = 0; i < 4; ++ i) {
        let idx = 0;
        for (let j = 0; j < 4; ++ j) {
          if (cellValue[j * 4 + i]) {
            cellValue[idx * 4 + i] = cellValue[j * 4 + i];
            idx ++;
          }
        }
        for (let j = idx; j < 4; ++ j) {
          cellValue[j * 4 + i] = 0;
        }
      }

      for(var i = 0; i < 16; i ++) {
        this.$set(this.values, i, cellValue[i]);
      }
    },
    moveRight () {
      var _this = this;
      for (let i = 0; i < 4; ++ i) {
        var idx = 3;
          for (let j = 3; j >= 0; -- j) {
            if (cellValue[i * 4 + j]) {
                cellValue[i * 4 + idx] = cellValue[i * 4 + j];
                idx --;
            }
          }
          for (let j = 0; j <= idx; ++ j) {
            cellValue[i * 4 + j] = 0;
          }
      }
      for (let i = 0; i < 4; ++ i) {
        for (let j = 0; j < 3; ++ j) {
          if (cellValue[i * 4 + j] == cellValue[i * 4 + j + 1]) {
            cellValue[i * 4 + j] *= 2;
            cellValue[i * 4 + j + 1] = 0;
          }
        }
      }
      for (let i = 0; i < 4; ++ i) {
        let idx = 3;
        for (let j = 3; j >= 0; -- j) {
            if (cellValue[i * 4 + j]) {
                cellValue[i * 4 + idx] = cellValue[i * 4 + j];
                idx --;
            }
        }
        for (let j = 0; j <= idx; ++ j) {
          cellValue[i * 4 + j] = 0;
        }
      }
      for(var i = 0; i < 16; i ++) {
        this.$set(this.values, i, cellValue[i]);
      }
    },
    generateMap() {
      var emptyGrid = [];
      for(var i = 0; i < 16; ++ i) {
        if (cellValue[i] == 0) {
          emptyGrid.push(i);
        }
      }
      if (emptyGrid.length >= 1) {
        var emptyLength = emptyGrid.length;
        var cellID = Math.floor(Math.random() * emptyLength);
        var value = 2;
        if (Math.random() < 0.3) {
          value = 4;
        }
        this.$set(this.values, emptyGrid[cellID],value);
      }
    }
  },
  created () {
    this.init();
    var _this = this;
    document.onkeydown = function (event) {
       if (event.keyCode==37) {
         _this.moveLeft();
         _this.generateMap();
       }//左
       if (event.keyCode==40) { //下
         _this.moveDown();
         _this.generateMap();
       }
       if (event.keyCode==39) { //右
         _this.moveRight();
         _this.generateMap();
       }
       if (event.keyCode==38) { //上
         _this.moveUp();
         _this.generateMap();
       }
       if (_this.isGameOver()) {
         alert("game over");
         _this.$set(_this.isGameOver, true);
       }
    }
  }
}
</script>

<style>
  .gameGrid {
    width: 500px;
    height: 500px;
    margin: auto;
    border: 10px solid #bbada0;
    border-radius: 10px;
    background-color: #bbada0;
    display: flex;
    flex-wrap: wrap;
  }

  .cell {
    width: 23%;
    height: 23%;
    background-color: rgba(238, 228, 218, 0.35);
    margin: auto;
    text-align: center;
    line-height: 115px;
    box-sizing: border-box;
    font-size: 54px;
    color: #FFF;
  }
</style>

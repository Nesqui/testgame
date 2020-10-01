<template>
  <div class="tag">
    <transition-group name="tag-group" class="tag-group">
      <button
        v-for="(cell, index) in cells"
        :key="cell"
        @click="move(index)"
        :class="cell ? 'cell' : 'cell-null'"
      >
        <span v-if="cell != 0">{{ cell }}</span>
      </button>
    </transition-group>
    <div class="rusult" v-if="winCondition">Победа</div>
    <button @click="win">Победить</button>
    <button @click="fillCells">Заново</button>
  </div>
</template>

<script>
export default {
  data: () => ({
    cells: [],
    dimension: 4,
  }),
  beforeMount() {
    this.fillCells();
  },
  methods: {
    win() {
      this.cells = [];
      for (let i = 1; i < this.dimension ** 2; i++) {
        this.cells.push(i);
      }
      this.cells.push(0);
    },
    fillCells() {
      this.cells = [];
      for (let i = 0; i < this.dimension ** 2; i++) {
        this.cells.push(i);
      }
      this.cells.sort(() => 0.5 - Math.random());
    },
    swap(index, nullIndex) {
      this.cells.splice(nullIndex, 1, this.cells[index]);
      this.cells.splice(index, 1, 0);
    },
    move(index) {
      if (this.cells[index + this.dimension] === 0)
        this.swap(index, index + this.dimension);
      else if (this.cells[index - this.dimension] === 0)
        this.swap(index, index - this.dimension);
      else if (this.cells[index + 1] === 0) this.swap(index, index + 1);
      else if (this.cells[index - 1] === 0) this.swap(index, index - 1);
    },
  },
  computed: {
    winCondition() {
      for (let i = 0; i < this.cells.length - 1; i++) {
        if (this.cells[i] !== i + 1) return false;
      }
      return true;
    },
  },
};
</script>

<style lang="scss" scoped>
$cellSize: 150px;
.tag {
  .tag-group {
    display: grid;
    grid-template-rows: $cellSize $cellSize $cellSize $cellSize;
    grid-template-columns: $cellSize $cellSize $cellSize $cellSize;
    .cell {
      background: rgb(75, 75, 75);
      color: white;
      font-size: 1.5rem;
    }
    .cell-null {
      background: none;
      border: none;
    }
    button:focus {
      outline: unset;
    }
  }
  .tag-group-move {
    transition: transform 0.1s;
  }
  .rusult {
    background-color: yellow;
    padding: 5px;
  }
}
</style>
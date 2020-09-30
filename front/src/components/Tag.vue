<template>
  <div class="tag">
    <div class="tag-wrapper">
      <div class="tag-horizontal" v-for="(horizontal, row) in cells" :key="row">
        <div class="cell" v-for="(cell, col) in horizontal" :key="col">
          <div @click="move(row, col)" v-if="cell" class="tag-cell">
            <span>
              {{ cell }}
            </span>
          </div>
          <div v-else class="cell-null"></div>
        </div>
      </div>
    </div>
    <div v-if="winCondition">
      <p>Победа</p>
      <button @click="fillCells">Еще</button>
    </div>
    <button @click="win">Выиграть</button>
  </div>
</template>

<script>
export default {
  name: "Tag",
  props: {
    dimension: {
      default: 3,
    },
  },
  data() {
    return {
      cells: [],
    };
  },
  beforeMount() {
    this.fillCells();
  },
  methods: {
    win() {
      this.cells = [
        [1, 2, 3],
        [4, 5, 6],
        [7, 8, 0],
      ];
    },
    move(row, col) {
      const swap = (rowOfNull, colOfNull) => {
        this.$set(this.cells[rowOfNull], colOfNull, this.cells[row][col]);
        this.$set(this.cells[row], col, 0);
      };
      if (this.cells[row + 1] && this.cells[row + 1][col] == 0)
        swap(row + 1, col);
      else if (this.cells[row - 1] && this.cells[row - 1][col] == 0)
        swap(row - 1, col);
      else if (this.cells[row][col + 1] == 0) swap(row, col + 1);
      else if (this.cells[row][col - 1] == 0) swap(row, col - 1);
    },
    fillCells() {
      this.cells = [];
      const pull = this.dimension ** 2;
      const cells = [];
      for (let i = 0; i < pull; i++) {
        cells.push(i);
      }
      cells.sort((a) => Math.random() * cells.length - a);
      for (let i = 0; i < this.dimension; i++) {
        this.cells.push(cells.splice(0, this.dimension));
      }
      console.log(this.cells);
    },
  },
  computed: {
    winCondition() {
      return !this.cells.find((cellX, row) =>
        cellX.find(
          (cell, col) => row * this.dimension + col + 1 !== this.cells[row][col]
        )
      );
    },
  },
};
</script>

<style scoped>
.tag {
  height: 100%;
}
.tag-wrapper {
  border: 15px solid black;
  display: flex;
  flex-direction: column;
  height: 100%;
}
.tag-horizontal {
  display: flex;
  height: 100%;
}
.cell {
  height: 100%;
  width: 100%;
}
.tag-cell {
  height: 100%;
  width: 100%;
  background-color: rgb(73, 73, 73);
  display: flex;
  justify-content: center;
  align-items: center;
  border: 2px solid darkgrey;
  cursor: pointer;
}
.cell-null {
  height: 100%;
  width: 100%;
  border: 2px solid darkgrey;
}
span {
  color: white;
  font-size: 72px;
}
</style>

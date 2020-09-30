<template>
  <div class="tag">
    <div class="tag-wrapper">
      <div class="tag-vertical" v-for="(horizontal, row) in cells" :key="row">
        <div
          class="tag-horizontal"
          v-for="(cell, col) in horizontal"
          :key="col"
        >
          <div @click="move(row, col)" v-if="cell" class="tag-cell">
            <span>
              {{ cell }}
            </span>
          </div>
        </div>
      </div>
    </div>
    <div v-if="winCondition">
      <p>Победа</p>
      <button @click="fillCells">Еще</button>
    </div>
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
        cellX.find((cell, col) => row * this.dimension + col + 1 !== this.cells[row][col])
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
  border: 2px solid black;
  display: flex;
  height: 100%;
  align-items: stretch;
  position: relative;
}
.tag-vertical {
  flex-grow: 1;
  display: flex;
  flex-direction: column;
  align-items: stretch;
}
.tag-horizontal {
  position: relative;
  height: 100%;
}
.tag-cell {
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: rgb(73, 73, 73);
  border: 2px solid darkgrey;
  border-radius: 2px;
  cursor: pointer;
}
span {
  color: white;
  font-size: 72px;
}
</style>

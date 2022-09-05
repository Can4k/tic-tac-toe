<template>
  <div id="app">
    <h1>KPESTIKI NOLIKI</h1>
      <table>
        <tr v-for="i in this.field.length">
          <td
              v-for="j in this.field && this.field[0].length"
              :class="{
                'win' : (winPositions.indexOf(this.getPositionIndex(i - 1, j - 1)) !== -1),
                'left-up' : (i === 1 && j === 1),
                'right-up' : (i === 1 && j === sizeOfField),
                'left-down' : (i === sizeOfField && j === 1),
                'right-down' : (i === sizeOfField && j === sizeOfField)
              }"
              @click="putSign(i - 1, j - 1)"
          >
            <transition name="fade">
              <img v-show="field[i - 1][j - 1] === 1" src="@/assets/x.svg" alt="X">
            </transition>
            <transition name="fade">
              <img class="O" v-show="field[i - 1][j - 1] === -1" src="@/assets/circle.svg" alt="O ">
            </transition>
          </td>
        </tr>
      </table>
    <div class="buttons-container">
      <transition name="fade">
        <button v-show="finished" @click="rebuild(3)">
          3 x 3
        </button>
      </transition>
      <transition name="fade">
        <button v-show="finished" @click="rebuild(4)">
          4 x 4
        </button>
      </transition>
      <transition name="fade">
        <button v-show="finished" @click="rebuild(5)">
          5 x 5
        </button>
      </transition>
    </div>
  </div>
</template>

<script>

const rules = {
  1: 1,
  2: 2,
  3: 3,
  4: 4,
  5: 4,
  6: 4,
  7: 5
}

export default {
  name: 'App',
  data() {
    return {
      field: [
          [0, 0, 0],
          [0, 0, 0],
          [0, 0, 0],
      ],
      winPositions : [],
      currentType: 1,
      finished: true,
      sizeOfField: 3,
      completed: 0
    }
  },
  methods: {
    putSign(i, j) {
      if (this.field[i][j] || this.finished) {
        return;
      }
      this.completed++;
      this.field[i][j] = this.currentType;
      this.currentType = (this.currentType === -1? 1 : -1);
      if (rules[this.sizeOfField] === 3) {
        this.finished = this.check3();
      } else {
        this.finished = this.check4();
      }
    },
    getPositionIndex(i, j) {
      return (i - 1) * this.sizeOfField + j - 1;
    },
    check3() {
      for (let i = 0; i < this.sizeOfField; i++) {
        for (let j = 0; j <= this.sizeOfField - rules[this.sizeOfField]; j++) {
          if (this.field[i][j] && this.field[i][j] === this.field[i][j + 1] && this.field[i][j + 1] === this.field[i][j + 2]) {
            this.winPositions.push(this.getPositionIndex(i, j));
            this.winPositions.push(this.getPositionIndex(i, j + 1));
            this.winPositions.push(this.getPositionIndex(i, j + 2));
            return true;
          }
          if (this.field[j][i] && this.field[j][i] === this.field[j + 1][i] && this.field[j + 1][i] === this.field[j + 2][i]) {
            this.winPositions.push(this.getPositionIndex(j, i));
            this.winPositions.push(this.getPositionIndex(j + 1, i));
            this.winPositions.push(this.getPositionIndex(j + 2, i));
            return true;
          }
          if (i + 2 < this.sizeOfField && j + 2 < this.sizeOfField && this.field[i][j] && this.field[i][j] === this.field[i + 1][j + 1] && this.field[i + 1][j + 1] === this.field[i + 2][j + 2]) {
            this.winPositions.push(this.getPositionIndex(i, j));
            this.winPositions.push(this.getPositionIndex(i + 1, j + 1));
            this.winPositions.push(this.getPositionIndex(i + 2, j + 2));
            return true;
          }
          if (i + 2 < this.sizeOfField && this.sizeOfField - j - 3 >= 0 && this.field[this.sizeOfField - j - 1][i] && this.field[this.sizeOfField - j - 1][i] === this.field[this.sizeOfField - j - 2][i + 1] && this.field[this.sizeOfField - j - 2][i + 1] === this.field[this.sizeOfField - j - 3][i + 2]) {
            this.winPositions.push(this.getPositionIndex(this.sizeOfField - j - 1, i));
            this.winPositions.push(this.getPositionIndex(this.sizeOfField - j - 2, i + 1));
            this.winPositions.push(this.getPositionIndex(this.sizeOfField - j - 3, i + 2));
            return true;
          }
        }
      }
      return this.completed === this.sizeOfField * this.sizeOfField;
    },
    check4() {
      for (let i = 0; i < this.sizeOfField; i++) {
        for (let j = 0; j <= this.sizeOfField - rules[this.sizeOfField]; j++) {
          if (this.field[i][j] && this.field[i][j] === this.field[i][j + 1] && this.field[i][j + 1] === this.field[i][j + 2] && this.field[i][j + 1] === this.field[i][j + 3]) {
            this.winPositions.push(this.getPositionIndex(i, j));
            this.winPositions.push(this.getPositionIndex(i, j + 1));
            this.winPositions.push(this.getPositionIndex(i, j + 2));
            this.winPositions.push(this.getPositionIndex(i, j + 3));
            return true;
          }
          if (this.field[j][i] && this.field[j][i] === this.field[j + 1][i] && this.field[j + 1][i] === this.field[j + 2][i] && this.field[j + 1][i] === this.field[j + 3][i]) {
            this.winPositions.push(this.getPositionIndex(j, i));
            this.winPositions.push(this.getPositionIndex(j + 1, i));
            this.winPositions.push(this.getPositionIndex(j + 2, i));
            this.winPositions.push(this.getPositionIndex(j + 3, i));
            return true;
          }
          if (i + 3 < this.sizeOfField && j + 3 < this.sizeOfField && this.field[i][j] && this.field[i][j] === this.field[i + 1][j + 1] && this.field[i + 1][j + 1] === this.field[i + 2][j + 2] && this.field[i + 1][j + 1] === this.field[i + 3][j + 3]) {
            this.winPositions.push(this.getPositionIndex(i, j));
            this.winPositions.push(this.getPositionIndex(i + 1, j + 1));
            this.winPositions.push(this.getPositionIndex(i + 2, j + 2));
            this.winPositions.push(this.getPositionIndex(i + 3, j + 3));
            return true;
          }
          if (i + 3 < this.sizeOfField && this.sizeOfField - j - 4 >= 0 && this.field[this.sizeOfField - j - 1][i] && this.field[this.sizeOfField - j - 1][i] === this.field[this.sizeOfField - j - 2][i + 1] && this.field[this.sizeOfField - j - 2][i + 1] === this.field[this.sizeOfField - j - 3][i + 2] && this.field[this.sizeOfField - j - 2][i + 1] === this.field[this.sizeOfField - j - 4][i + 3]) {
            this.winPositions.push(this.getPositionIndex(this.sizeOfField - j - 1, i));
            this.winPositions.push(this.getPositionIndex(this.sizeOfField - j - 2, i + 1));
            this.winPositions.push(this.getPositionIndex(this.sizeOfField - j - 3, i + 2));
            this.winPositions.push(this.getPositionIndex(this.sizeOfField - j - 4, i + 3));
            return true;
          }
        }
      }
      return this.completed === this.sizeOfField * this.sizeOfField;
    },
    rebuild(sz) {
      this.field = [];
      this.sizeOfField = sz;
      for (let i = 0; i < this.sizeOfField; i++) {
        this.field.push(Array(this.sizeOfField));
      }
      this.winPositions = [];
      this.finished = false;
      this.currentType = 1;
      this.completed = 0;
    }
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: black;
  margin-top: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}
table {
  margin-top: 26px;
  border: 1px solid black;
  background-color: black;
  border-radius: 8px;
  transition-duration: .3s;
}
tr {
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 10px;
}
td {
  border: 1px solid black;
  background-color: white;
  display: flex;
  justify-content: center;
  align-items: center;
  transition-duration: 1s;
  width: 60px;
  height: 60px;
}
img {
  width: 60px;
  transition-duration: .3s;
}
.O {
  width: 48px;
  margin: 6px;
}
h1 {
  font-size: 40px;
  font-family: 'Lexend Exa', sans-serif;
}
button {
  font-family: 'Lexend Exa', sans-serif;
  font-size: 20px;
  border-radius: 5px;
  background-color: white;
  border: 1px solid black;
  padding: 5px;
  transition-duration: 1s;
  cursor: pointer;
  margin: 10px;
}
button:hover {
  background-color: yellow;
  box-shadow: 0 0 10px -7px yellow;
}
.buttons-container {
  margin-top: 25px;
  display: flex;
  align-items: center;
  justify-content: center;
}
.win {
  background-color: yellow;
}
.left-up {
  border-radius: 6px 0 0 0;
}
.left-down {
  border-radius: 0 0 0 6px;
}
.right-up {
  border-radius: 0 6px 0 0;
}
.right-down {
  border-radius: 0 0 6px 0;
}
.fade-enter-active,
.fade-leave-active {
  transform: scale(1);
}
.fade-enter-from,
.fade-leave-to {
  transform: scale(0);
}
</style>

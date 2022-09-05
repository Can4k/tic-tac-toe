<template>
  <div id="app">
    <h1>tic-tac-toe</h1>
      <table>
        <tr v-for="i in 3">
          <td
              v-for="j in 3"
              :class="{
                'win' : (winPositions.indexOf(this.getPos(i - 1, j - 1)) !== -1),
                'left-up' : (i === 1 && j === 1),
                'right-up' : (i === 1 && j === 3),
                'left-down' : (i === 3 && j === 1),
                'right-down' : (i === 3 && j === 3)
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
    <transition name="fade">
      <button v-show="finished" @click="rebuild">
        Play again
      </button>
    </transition>
  </div>
</template>

<script>
export default {
  name: 'App',
  components: {

  },
  data() {
    return {
      field: [
          [0, 0, 0],
          [0, 0, 0],
          [0, 0, 0]
      ],
      winPositions : [],
      currentType: 1,
      finished: false
    }
  },
  methods: {
    putSign(i, j) {
      if (this.field[i][j] || this.finished) {
        return;
      }
      this.complited++;
      this.field[i][j] = this.currentType;
      this.currentType = (this.currentType === -1? 1 : -1);
      this.finished = this.check();
    },
    getPos(i, j) {
      return (i - 1) * 3 + j - 1;
    },
    check() {
      for (let i = 0; i < 3; i++) {
        if (this.field[i][0] && this.field[i][0] === this.field[i][1] && this.field[i][1] === this.field[i][2]) {
          this.winPositions.push(this.getPos(i, 0));
          this.winPositions.push(this.getPos(i, 1));
          this.winPositions.push(this.getPos(i, 2));
          console.table(this.winPositions);
          return true;
        }
        if (this.field[0][i] && this.field[0][i] === this.field[1][i] && this.field[1][i] === this.field[2][i]) {
          this.winPositions.push(this.getPos(0, i));
          this.winPositions.push(this.getPos(1, i));
          this.winPositions.push(this.getPos(2, i));
          return true;
        }
      }
      if (this.field[0][0] && this.field[0][0] === this.field[1][1] && this.field[1][1] === this.field[2][2]) {
        this.winPositions.push(this.getPos(0, 0));
        this.winPositions.push(this.getPos(1, 1));
        this.winPositions.push(this.getPos(2, 2));
        return true;
      }
      if (this.field[0][2] && this.field[0][2] === this.field[1][1] && this.field[1][1] === this.field[2][0]) {
        this.winPositions.push(this.getPos(0, 2));
        this.winPositions.push(this.getPos(1, 1));
        this.winPositions.push(this.getPos(2, 0));
        return true;
      }
      return this.complited === 9;
    },
    rebuild() {
      this.field = [
        [0, 0, 0],
        [0, 0, 0],
        [0, 0, 0]
      ];
      this.winPositions = [];
      this.finished = false;
      this.currentType = 1;
      this.complited = 0;
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
  margin-top: 60px;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}
table {
  border: 1px solid black;
  background-color: black;
  border-radius: 8px;
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
  font-size: 70px;
  font-family: 'Lexend Exa', sans-serif;
}
button {
  margin-top: 20px;
  font-family: 'Lexend Exa', sans-serif;
  font-size: 20px;
  border-radius: 5px;
  background-color: white;
  border: 1px solid black;
  padding: 3px;
  transition-duration: 1s;
  cursor: pointer;
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

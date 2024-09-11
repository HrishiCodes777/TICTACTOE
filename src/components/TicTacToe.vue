<template>
  <div class="tic-tac-toe">
    <h1>Tic Tac Toe</h1>
    <div class="board">
      <div class="row" v-for="(row, rowIndex) in board" :key="rowIndex">
        <div
          class="cell"
          v-for="(cell, cellIndex) in row"
          :key="cellIndex"
          :class="{ 'cell-x': cell === 'X', 'cell-o': cell === 'O' }"
          @click="makeMove(rowIndex, cellIndex)"
        >
          {{ cell }}
        </div>
      </div>
    </div>
    <p v-if="winner">{{ winner }} wins!</p>
    <p v-else-if="isTie">It's a tie!</p>
    <button @click="resetGame">Reset Game</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      board: [
        ['', '', ''],
        ['', '', ''],
        ['', '', '']
      ],
      currentPlayer: 'X',
      winner: null,
      isTie: false
    };
  },
  methods: {
    makeMove(row, col) {
      if (!this.board[row][col] && !this.winner) {
        this.$set(this.board[row], col, this.currentPlayer);
        if (this.checkWin()) {
          this.winner = this.currentPlayer;
        } else if (this.checkTie()) {
          this.isTie = true;
        } else {
          this.currentPlayer = this.currentPlayer === 'X' ? 'O' : 'X';
        }
      }
    },
    checkWin() {
      const winningCombinations = [
        [[0, 0], [0, 1], [0, 2]],
        [[1, 0], [1, 1], [1, 2]],
        [[2, 0], [2, 1], [2, 2]],
        [[0, 0], [1, 0], [2, 0]],
        [[0, 1], [1, 1], [2, 1]],
        [[0, 2], [1, 2], [2, 2]],
        [[0, 0], [1, 1], [2, 2]],
        [[0, 2], [1, 1], [2, 0]]
      ];

      return winningCombinations.some(combination => {
        const [a, b, c] = combination;
        return (
          this.board[a[0]][a[1]] &&
          this.board[a[0]][a[1]] === this.board[b[0]][b[1]] &&
          this.board[a[0]][a[1]] === this.board[c[0]][c[1]]
        );
      });
    },
    checkTie() {
      return this.board.flat().every(cell => cell);
    },
    resetGame() {
      this.board = [
        ['', '', ''],
        ['', '', ''],
        ['', '', '']
      ];
      this.currentPlayer = 'X';
      this.winner = null;
      this.isTie = false;
    }
  }
};
</script>

<style>
.tic-tac-toe {
  text-align: center;
  font-family: 'Arial', sans-serif;
  margin-top: 50px;
}

h1 {
  font-size: 2em;
  color: #333;
}

.board {
  display: inline-block;
  margin-top: 20px;
  border: 2px solid #333;
  border-radius: 10px;
  overflow: hidden;
}

.row {
  display: flex;
}

.cell {
  width: 60px;
  height: 60px;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 1px solid #bbb;
  cursor: pointer;
  font-size: 2em;
  transition: background-color 0.3s;
}

.cell:hover {
  background-color: #f0f0f0;
}

.cell-x {
  color: #f00;
}

.cell-o {
  color: #00f;
}

button {
  display: block;
  margin: 20px auto 0;
  font-size: 1em;
  padding: 10px 20px;
  border-radius: 5px;
  background-color: #333;
  color: #fff;
  border: none;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #555;
}

p {
  font-size: 1.2em;
  color: #333;
}
</style>

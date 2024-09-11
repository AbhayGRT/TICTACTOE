<template>
  <div class="tic-tac-toe">
    <h1>Tic Tac Toe</h1>
    <div class="board">
      <div 
        v-for="cell in board" 
        :key="cell.index" 
        :class="['cell', { 'x': cell.value === 'X', 'o': cell.value === 'O' }]"
        @click="handleCellClick(cell.index)"
      >
        {{ cell.value }}
      </div>
    </div>
    <p v-if="winner" class="status winner">{{ winner }} wins!</p>
    <p v-else-if="isDraw" class="status draw">It's a draw!</p>
    <p v-else class="status">Current player: {{ currentPlayer }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      board: Array(9).fill().map((_, index) => ({ index, value: null })),
      currentPlayer: 'X',
      winner: null,
      isDraw: false,
    };
  },
  methods: {
    handleCellClick(index) {
      if (this.board[index].value || this.winner) return;
      this.board[index].value = this.currentPlayer;
      this.checkWinner();
      this.currentPlayer = this.currentPlayer === 'X' ? 'O' : 'X';
    },
    checkWinner() {
      const winningCombinations = [
        [0, 1, 2], [3, 4, 5], [6, 7, 8],
        [0, 3, 6], [1, 4, 7], [2, 5, 8],
        [0, 4, 8], [2, 4, 6]
      ];
      for (const combination of winningCombinations) {
        if (
          this.board[combination[0]].value &&
          this.board[combination[0]].value === this.board[combination[1]].value &&
          this.board[combination[1]].value === this.board[combination[2]].value
        ) {
          this.winner = this.board[combination[0]].value;
          return;
        }
      }
      if (this.board.every(cell => cell.value !== null)) {
        this.isDraw = true;
      }
    },
  },
};
</script>

<style scoped>
.tic-tac-toe {
  display: flex;
  flex-direction: column;
  align-items: center;
  font-family: Arial, sans-serif;
  margin-top: 50px;
}

h1 {
  margin-bottom: 20px;
  color: #333;
}

.board {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 10px;
  background-color: #f0f0f0;
  padding: 15px;
  border-radius: 10px;
}

.cell {
  width: 80px;
  height: 80px;
  background-color: white;
  border: 2px solid #ddd;
  border-radius: 5px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 36px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.cell:hover {
  background-color: #f9f9f9;
}

.cell.x {
  color: #e74c3c;
}

.cell.o {
  color: #3498db;
}

.status {
  margin-top: 20px;
  font-size: 24px;
  font-weight: bold;
}

.winner {
  color: #27ae60;
}

.draw {
  color: #f39c12;
}
</style>
<template>
  <div>
    <p class="mb-2 text-lg text-white">
      {{ currentPlayer }}'s turn
    </p>
    <div
    class="flex flex-row"
    v-for="(row, rowIndex) in board"
    :key="rowIndex">
      <button
      @click="handleCellClick(rowIndex, cellIndex)"
      class="text-xl text-white border border-white w-36 h-36"
        v-for="(cell, cellIndex) in row"
        :key="cellIndex">
        {{ cell }}
      </button>
    </div>
    <p class="mt-2 text-red-500">{{ error }}</p>
  </div>
  <div v-if="isWinned === true || isTie === true">
    <WinnerModal :winner="currentPlayer" :tied="isTie" :callback="onRematch" />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import WinnerModal from './WinnerModal.vue';

export default defineComponent({
  components: {
    WinnerModal,
  },
  data() {
    return {
      board: [
        ['', '', ''],
        ['', '', ''],
        ['', '', ''],
      ],
      isWinned: false,
      isTie: false,
      error: '',
      currentPlayer: 'X',
    };
  },
  methods: {
    handleCellClick(rowIndex: number, cellIndex: number) {
      if (this.board[rowIndex][cellIndex] === '') {
        this.board[rowIndex][cellIndex] = this.currentPlayer;
        const isWinned = this.checkWinner();
        if (!isWinned) {
          this.currentPlayer = this.currentPlayer === 'X' ? 'O' : 'X';
        }
      } else {
        this.error = 'This cell already has value';
      }
    },

    checkWinner() {
      const isColsSame = this.checkCols();
      const isRowsSame = this.checkRows();
      const isDiagonalsSame = this.checkDiagonals();

      if (isColsSame || isRowsSame || isDiagonalsSame) {
        this.isWinned = true;
      }

      const isTied = this.checkTie();

      return this.isWinned || isTied;
    },

    checkCols() {
      let win = false;
      for (let row = 0; row < this.board.length; row += 1) {
        const arr: string[] = [];
        if (!win) {
          for (let cell = 0; cell < this.board[row].length; cell += 1) {
            arr.push(this.board[cell][row]);
          }
          if (!arr.includes('')) {
            win = arr.every((item) => item === arr[0]);
          }
        }
      }
      return win;
    },

    checkRows() {
      let win = false;
      for (let row = 0; row < this.board.length; row += 1) {
        if (!this.board[row].includes('') && !win) {
          win = this.board[row].every((item) => item === this.board[row][0]);
        }
      }
      return win;
    },

    checkDiagonals() {
      const firstDiagonal: string[] = [];
      const secondDiagonal: string[] = [];

      for (let row = 0; row < this.board.length; row += 1) {
        firstDiagonal.push(this.board[row][row]);
        secondDiagonal.push(this.board[row][this.board.length - row - 1]);
      }
      return firstDiagonal.every((item) => firstDiagonal[0] === item && item !== '') || secondDiagonal.every((item) => secondDiagonal[0] === item && item !== '');
    },

    checkTie() {
      const existingItems = this.board.filter((item) => item.includes(''));
      if (!existingItems.length && !this.isWinned) {
        this.isTie = true;
      }
      return this.isTie;
    },

    onRematch() {
      this.board = [
        ['', '', ''],
        ['', '', ''],
        ['', '', ''],
      ];
      this.error = '';
      this.isWinned = false;
      this.currentPlayer = 'X';
      this.isTie = false;
    },
  },
});

</script>

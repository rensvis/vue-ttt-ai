<template>
  <div class="board-container">
    <div id="board" class="board" :class="[aiPlaying ? 'disabled': '', turn === 'X' ? 'X' : 'O', !inProgress ? 'fade': '']">
      <Tile v-for="(tile, i) in board" :key="i" @click="tileClick(i)" :playedBy="tile"/>

      <!-- winning line -->
      <WinningLine :class="{show : winner !== '' && !inProgress}" :winningCombination="winningCombination" />
    </div>
    <WinningMessage :class="{show: !inProgress}" :winner="winner" @play-again="playAgain" />
  </div>
</template>

<script>

import Tile from "./Tile.vue";
import WinningMessage from "./WinningMessage.vue";
import WinningLine from "./WinningLine.vue";

export default {
  name: "Board",
  components: {
    Tile,
    WinningMessage,
    WinningLine,
  },
  props: {
    difficulty: String,
    scores: Object,
  },
  data () {
    return {
      board: Array(9).fill(''),
      winningCombinations: [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6],
      ],
      turn: "X", // X starts
      roles: {
        human: "X",
        ai: "O"
      }, // default
      inProgress: true,
      winner: "",
      winningCombination: null,
      aiPlaying: false, // waiting for ai to finsih turn
    }
  },
  created () {
    // this.winningLine = document.querySelector('.winning-line');
  },
  methods: {

    tileClick(i) {
      // check valid
      if (!this.inProgress || this.board[i] !== "" || this.aiPlaying) return;

      // make move
      const result = this.makeMove(i);
      if (result) return;
      // ai turn
      if (this.inProgress) {
        this.aiPlaying = true;
        setTimeout(() => {
          this.AITurn();
        }, 10);
      };

    },

    makeMove(i) {
      // add mark
      this.board = this.board.map((t, index) => (i === index) ? this.turn: t);
      // check winner
      const result = this.checkWinner(this.board, this.turn)
      if (result) {
        this.endGame(result);
        return result;
      }

      // switch turns
      this.turn = (this.turn === "X") ? "O": "X";

    },

    AITurn() {
      let board = this.board,
        moves = [];
      let pickedMove;

      if (this.difficulty === "blind") { // pick random move
        moves = board.reduce((filtered, t, i) => {
          if (t === "") filtered.push(i);
          return filtered;
        }, []);
        pickedMove = moves[Math.floor(Math.random() * moves.length)]
      } else { // use minimax to find best move
        // let bestScore = Infinity // AI is minimizing
        for (let i = 0; i < board.length; i++) {
          if (board[i] === "") {
            board[i] = "O";
            let score = this.minimax(board, 0, true, i); // first next move, this is X and X is maximizing
            board[i] = "";
            moves.push({
              move: i,
              score: score
            });
          }
        }
        // order moves by score
        moves.sort(function (a, b) {
          return a.score - b.score;
        })

        // functions to get random move from all with best or second best score, to prevent playing the same game every time
        const randomBestMove = (moves) => {
          const bestMoves = moves.filter(move => move.score === moves[0].score);
          console.log(bestMoves[Math.floor(Math.random() * bestMoves.length)].move);
          return bestMoves[Math.floor(Math.random() * bestMoves.length)].move;
        }
        const randomNextBestMove = (moves) => {
          const bestMoves = moves.filter(move => move.score !== moves[0].score);
          return bestMoves[Math.floor(Math.random() * bestMoves.length)].move;
        }

        if (this.difficulty === "impossible") { // pick best move
          pickedMove = randomBestMove(moves);
        } else { // pick best move 75% of the time
          pickedMove = (Math.random() < .75) ? randomBestMove(moves) : randomNextBestMove(moves);
        }
      }

      setTimeout(() => {
        this.makeMove(pickedMove);
        this.aiPlaying = false;
      }, 600);
    }, // End AITurn


    minimax(board, depth, isMaximizing, firstMove) {
      // const player = (isMaximizing) ? "X" : "O"
      const result = this.checkWinner(board)
      if (result) { // state terminal, return score
        let scores = {
          X: 10 - depth,
          O: -10 + depth,
          tie: 0
        }
        return scores[result];
      }

      if (isMaximizing) { // maximizing
        let bestScore = -Infinity;
        for (let i = 0; i < board.length; i++) {
          if (board[i] === "") { // is spot availbale?
            board[i] = "X";
            let score = this.minimax(board, depth + 1, false, firstMove);
            board[i] = "";
            if (score > bestScore) {
              bestScore = score;
            }
          }
        }
        return bestScore;
      } else { // minimizing
        let bestScore = Infinity;
        for (let i = 0; i < board.length; i++) {
          if (board[i] === "") { // is spot availbale?
            board[i] = "O";
            let score = this.minimax(board, depth + 1, true, firstMove);
            board[i] = "";
            if (score < bestScore) {
              bestScore = score;
            }
          }
        }
        return bestScore;
      }
    },

    checkWinner(board = this.board) {
      const B = board // easier reading
      for (let i = 0; i < this.winningCombinations.length; i++) {
        const comb = this.winningCombinations[i]
        if (B[comb[0]] == B[comb[1]] && B[comb[1]] == B[comb[2]] && B[comb[0]] != "") {
          const winner = B[comb[0]]
          this.winningCombination = i
          return winner
        }
      }
      // check draw
      if (B.filter(t => t === "").length === 0) {
        return "tie"
      } else {
        return false
      }
    }, // End checkWinner

    endGame(result) {
      if (result === "tie") this.winningCombination = null;
      this.winner = result;
      this.inProgress = false;
      this.$emit('update-result', result);
    }, // End endGame


    playAgain() {
      this.board = Array(9).fill('');
      this.turn = "X";
      this.inProgress = true;
      this.$emit('new-round');
      setTimeout(() => {
        this.winner = ""; // delay for winning message transition
      }, 500);
    },



  }

};
</script>





<style lang="scss" scoped>
.board-container {
  position: relative;
}
#board {
  height: 300px;
  width: 300px;
  margin: auto;
  padding: -1px;
  display: grid;
  background: transparent;
  border-radius: 5px;
  overflow: hidden;
  gap: 0px;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(3, 1fr);


  @media only screen and (max-width: 350px) {
    height: 280px;
    max-width: 280px;
  }


  // animations
  transform: scale(1);
  opacity: 1;
  transition: opacity .2s, transform .6s;

  &.fade {
    transform: scale(.8);
    opacity: .1;
    transition-delay: .6s;
  }

}

</style>

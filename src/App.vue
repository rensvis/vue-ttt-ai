<template>
  <div class='header'>
    <h1>Round <span class='round-count'>{{roundCount}}</span></h1>
    <Score :scores="scores" :userName="userName" @update-username="updateUsername" />
    <Difficulty :difficulty="difficulty" @change-difficulty="changeDifficulty"/>
  </div>
  <div class='board-container'>
    <Board :difficulty="difficulty" :scores="scores" :inProgress="inProgress" @update-result="updateResult" @new-round="newRound" />
  </div>
  <div id="explanation" class="container">
    <p>Take a chance at beating this Vue Tic Tac Toe bot!</p>
    <p>Start a new game by making the first move. If you feel like the AI is a bit too powerfull just select a different difficulty level. For a more personal touch, add your username!</p>
    <p>This project is built with <i class="fab fa-vuejs"></i> Vue.js. Check out the code on <a href="https://github.com/rensvis/vue-ttt-ai"><i class="fab fa-github"></i> GitHub</a>.</p>
    <button @click="clearHistory()" id="clear-history">Clear history</button>
  </div>
</template>

<script>
import Board from "./components/Board.vue";
import Score from "./components/Score.vue";
import Difficulty from "./components/Difficulty.vue";

export default {
  name: "App",
  components: {
    Board,
    Score,
    Difficulty,
  },
  data() {
    return {
      difficulty: "impossible",
      scores: {
        human: 0,
        ai: 0
      },
      roundCount: 1,
      userName: "You"
    }
  },
  created () {
    this.difficulty = localStorage.getItem('difficulty') || "impossible";
    this.roundCount = localStorage.getItem('roundCount') || 1;
    this.scores.human = localStorage.getItem('scoreHuman') || 0;
    this.scores.ai = localStorage.getItem('scoreAi') || 0;
    this.userName = localStorage.getItem('userName') || "You";
  },
  methods: {
    changeDifficulty(lvl) {
      this.difficulty = lvl;
      localStorage.setItem("difficulty", lvl)
    },

    updateResult(result) {
      if (result === "X") {
        this.scores.human++
      } else if (result === "O") {
        this.scores.ai++
      }

      localStorage.setItem("scoreHuman", this.scores.human)
      localStorage.setItem("scoreAi", this.scores.ai)
    },

    newRound() {
      this.roundCount++;
      localStorage.setItem("roundCount", this.roundCount)
    },

    clearHistory() {
      localStorage.setItem("roundCount", 0);
      localStorage.setItem("scoreHuman", 0);
      localStorage.setItem("scoreAi", 0)
      this.roundCount = 1;
      this.scores.human = 0;
      this.scores.ai = 0;
    },

    updateUsername(userName) {
      this.userName = (userName !== "") ? userName: "You";
      localStorage.setItem("userName", this.userName);

    }
  },
};
</script>

<style lang="scss">
#app {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 6rem 0;
}

h1 {
  font-size: 3rem;
}

.header {
  text-align: center;
  width: 100%;
}

#explanation {
  margin-top: 5rem;
}

#clear-history {
  margin-top: 1.5rem;
  // border: none;
  padding: .5rem .7rem;
  font-size: 1rem;
}
</style>

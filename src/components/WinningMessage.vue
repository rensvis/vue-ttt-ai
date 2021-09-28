<template>
  <div class='winning-message'>
    <h2 class='winning-message__title'>{{header}}</h2>
    <p class='winning-message__joke'>{{joke}}</p>
    <button class='winning-message__btn' @click="onClickPA()">Play again</button>
  </div>
</template>

<script>

export default {
  name: "WinningMessage",
  components: {
  },
  props: {
    winner: String,
  },
  data() {
    return {
    }
  },
  computed: {
    joke: function () {
      const messageJokes = {
        X: [
          "Too easy for you?",
          "You beat AI! Maybe there's hope for humanity. :)",
          "Are you a tic tac toe master by any chance?",
          "Winning isn’t everything, it’s the only thing.",
          "Winning solves everything.",
        ],
        O: [
          "Why does Artificial Intelligence have to be so intelligent..?",
          "Don't worry, you'll get him next time.",
          "He must get tired at some point, right?",
          "Losing feels worse than winning feels good.",
          "You can’t win unless you learn how to lose.",
          "You win some, you lose some.",
          "Don't worry, he's cheating by using AI."

        ],
        draw: [
          "Didn't win, but didn't lose either!",
          "Practice round, catch him on the next one.",
          "Winners never quit, quiters never win.",
          "Persistence is key.",
          "Sometimes you win and sometimes you learn.",
          "Winning isn’t everything, but wanting it is.",
        ]
      }
      if (this.winner === "X") {
        return messageJokes.X[Math.floor(Math.random() * messageJokes.X.length)]
      } else if (this.winner === "O") {
        return messageJokes.O[Math.floor(Math.random() * messageJokes.O.length)]
      } else {
        return messageJokes.draw[Math.floor(Math.random() * messageJokes.O.length)]
      }

    },
    header: function () {
      // if (this.winner === "X") {
      //   return messageJokes.X[Math.floor(Math.random() * messageJokes.X.length)]
      // } else if (this.winner === "O") {
      //   return messageJokes.O[Math.floor(Math.random() * messageJokes.O.length)]
      // } else {
      //   return messageJokes.draw[Math.floor(Math.random() * messageJokes.O.length)]
      // }
      return (this.winner === "tie") ? "Draw.": `${this.winner} won!`;
    }
  },
  methods: {
    onClickPA() {
      this.$emit('play-again');
    }
  },
};
</script>

<style lang="scss" scoped>
    .winning-message {
      position: absolute;
      top: 50%;
      left: 50%;
      width: 100%;
      text-align: center;
      color: white;
      opacity: 0;
      transition: opacity .2s, transform .6s;
      transform: translate(-50%, -50%) scale(.8);
      z-index: -1;

      &.show {
        transform: translate(-50%, -50%) scale(1);
        transition-delay: .5s;
        opacity: 1;
        z-index: auto;
      }

      &__title {
        font-size: 2rem;
      }

      &__joke {
        margin-bottom: 1.5rem;
        font-size: 1.2rem;
      }

      &__btn {
        transition: all .2s;
        position: relative;

        &:after {
          position: absolute;
          content: '';
          top: 100%;
          bottom: 0;
          left: 0;
          right: 0;
          transition: all .2s;
          background: var(--body);
          z-index: -1;
        }

        &:hover {
          color: var(--bg);

          &:after {
            top: 0;
          }
        }
      }
    }
</style>

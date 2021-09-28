<template>
  <div class="scores">
    <div class="scores__player">
      <div class="scores__name"><i class="fas fa-user"></i><input type="text" ref="input" :value="userName" @input="onInput" @change="onChange"><span class='scores__score scores__score--user'>{{scores.human}}</span></div>
    </div>
    <div class="scores__vs"><span>vs</span></div>
    <div class="scores__ai">
      <span class='scores__score scores__score--robot'>{{scores.ai}}</span><span class="scores__name">Robot</span><i class="fas fa-robot"></i>
    </div>
  </div>
</template>

<script>

export default {
  name: "Score",
  components: {
  },
  props: {
    scores: Object,
    userName: String,
  },
  data() {
    return {
      content: ""
    }
  },
  mounted() {
    this.updateInputWidth();
  },
  methods: {
    onInput(e) {
      const maxLength = 15;
      if (e.target.value.length > maxLength) e.target.value = e.target.value.substring(0, maxLength); // max 20 chars
      this.updateInputWidth();
    },
    onChange(event) {
      this.$emit('update-username', event.target.value)
    },
    updateInputWidth() {
      const input = this.$refs["input"];
      console.log(this.$refs);
      input.style.width = input.value.length + "ch";
    }
  }
};
</script>

<style lang="scss" scoped>
  @mixin atMedium {
    @media (max-width: 1000px) {
      @content;
    }
  }

  $iconMargin: 1.5rem;

  .scores {
    display: flex;
    width: 100%;
    justify-content: center;
    margin-bottom: 1rem;
    font-size: 1.5rem;

    &__name {

      input {
        background: transparent;
        border: none;
        // border-bottom: 1px solid white;
        border-bottom: 1px solid transparent;
        font-size: 1.5rem;
        font-family: var(--font);
        width: fit-content;
        color: var(--body);
        min-width: 3ch;
        // padding-right: ;
        // border-radius: .25rem;
        &:hover {
          // outline: solid 1px;
          border-bottom: 1px solid white;
        }
      }
    }

    &__player, &__ai {
      flex: 1;
    }

    &__player {
      text-align: right;

      i {
        margin-right: $iconMargin;
      }
    }

    &__ai {
      text-align: left;
      flex: 1;

      i {
        margin-left: $iconMargin;
      }
    }

     &__vs {
        width: 60px;
        // font-size: .8rem;
        font-style: italic;
        span {
          display: block;
          transform: scale(0.7);
        }
     }

    &__score {
      transform: scale(1);
      &.pulse {
        animation: pulse .5s;
      }

      &--user {
        color: var(--primary);
        margin-left: $iconMargin;
      }
      &--robot {
        color: var(--secondary);
        margin-right: $iconMargin;
      }
    }

    // smaller screens
    @include atMedium {
      flex-direction: column;
      width: fit-content;
      margin: auto;
      margin-bottom: 2rem;

      .scores {

        &__vs {
          display: none;
        }
        &__player {
          text-align: left;
          margin-bottom: .8rem;
        }

        &__ai {
          display: flex;

          i {
            order: 1;
            margin-left: 0;
            margin-right: $iconMargin;
          }
          .scores__name {order: 2;}
          .scores__score {
            order: 3;
            margin-right: 0;
            margin-left: $iconMargin;
          }
        }
      }
    }
  }

  @keyframes pulse {
    0% { transform: scale(1)}
    50% { transform: scale(2) }
    100% { transform: scale(1) }
  }
</style>

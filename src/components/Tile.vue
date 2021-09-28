<template>
  <div :class="[playedBy]" class="tile"></div>
</template>

<script>

export default {
  name: "Tile",
  components: {
  },
  props: {
    playedBy: {
      type: String,
      default: ""
    }
  },
  data() {
    return {
      content: ""
    }
  }
};
</script>

<style lang="scss" scoped>

$x: var(--primary);
$o: var(--secondary);

.tile {
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: xx-large;

  /* borders */
  &:not(:nth-child(3n)) {
    border-right: solid white 3px;
  }
  &:not(:nth-child(3n - 2)) {
    border-left: solid white 3px;
  }
  &:not(:nth-child(n + 7)) {
    border-bottom: solid white 3px;
  }
  &:not(:nth-child(-n + 3)) {
    border-top: solid white 3px;
  }
}


/* X and O logic */
// X
.tile.X::before,
.tile.X::after,
.board.X:not(.disabled) .tile:not(.X):not(.O):hover::before,
.board.X:not(.disabled) .tile:not(.X):not(.O):hover::after {
  content: '';
  position: absolute;
  width: 50px;
  height: 10px;
  background-color: $x;
}
.tile.X::before,
.board.X .tile:not(.X):not(.O):hover::before {
  transform: rotate(45deg)
}
.tile.X::after,
.board.X .tile:not(.X):not(.O):hover::after {
  transform: rotate(-45deg)
}

// O
.tile.O::before,
.board.O:not(.disabled) .tile:not(.X):not(.O):hover::before {
  content: '';
  position: absolute;
  border-radius: 50%;
}
.tile.O::before,
.board.O .tile:not(.X):not(.O):hover::before {
  width: 30px;
  height: 30px;
  border: 10px solid $o;
  animation: fadein .2s;

}

/* hover styles */
.board.X .tile:not(.X):not(.O):hover::before,
.board.X .tile:not(.X):not(.O):hover::after {
  background: var(--bg-offset) !important;
  background: var(--primary-offset) !important;
}
.board.O:not(.disabled) .tile:not(.X):not(.O):hover::before {
  border: 10px solid rgb(85, 85, 85) !important;
}
/* pointer when tile is available and game is not over */
.board:not(.disabled) .tile:not(.O):not(.X) {
  cursor: pointer;
}

.tile.X, .tile.O {
  cursor: auto;
}
</style>

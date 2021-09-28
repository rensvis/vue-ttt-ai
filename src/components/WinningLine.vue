<template>
  <svg class='winning-line'>
    <line :x1='coords.x1' :y1='coords.y1' :x2='coords.x2' :y2='coords.y2'/>
  </svg>
</template>

<script>

export default {
  name: "WinningLine",
  components: {
  },
  props: {
    winningCombination: Number,
  },
  data() {
    return {
    }
  },
  computed: {
    coords: function () {
      if (!this.winningCombination && this.winningCombination !== 0) return {
        x1: 0,
        y1: 0,
        x2: 0,
        y2: 0,
      };
      const coordsTable = { // linked to winningCombinations array
        0: [
          [0, 50],
          [300, 50]
        ],
        1: [
          [0, 150],
          [300, 150]
        ],
        2: [
          [0, 250],
          [300, 250]
        ],
        3: [
          [50, 0],
          [50, 300]
        ],
        4: [
          [150, 0],
          [150, 300]
        ],
        5: [
          [250, 0],
          [250, 300]
        ],
        6: [
          [0, 0],
          [300, 300]
        ],
        7: [
          [300, 0],
          [0, 300]
        ]
      }
      console.log(this.winningCombination);
      const coords = coordsTable[this.winningCombination];
      return {
        x1: coords[0][0],
        y1: coords[0][1],
        x2: coords[1][0],
        y2: coords[1][1],
      }
    }
  },
  methods: {
  },
};
</script>


<style lang="scss" scoped>
.winning-line {
  position: absolute;
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;
  z-index: 0;
  visibility: hidden;

  &.show {
    visibility: visible;
    line {
      animation: draw .5s forwards;
    }
  }
  line {
    stroke: white;
    stroke-width: 4px;
    stroke-dasharray: 450;
    border-radius: 5px;
    stroke-dashoffset: 450;
    stroke-linecap: round;
  }
}


@keyframes draw {
  100% { stroke-dashoffset: 0; }
}
</style>

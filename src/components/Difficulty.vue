<template>
   <table class='difficulty'>
      <tbody>
        <tr class='difficulty__row'>
          <td class='difficulty__cell' style=''>
            <button @click="onChangeDifficulty('blind')" :class="{active: difficulty === 'blind'}" class='difficulty__btn'>Blind</button>
          </td>
          <td class='difficulty__cell' style=''>
            <button @click="onChangeDifficulty('medium')" :class="{active: difficulty === 'medium'}" class='difficulty__btn'>Medium</button>
          </td>
          <td class='difficulty__cell' style=''>
            <button @click="onChangeDifficulty('impossible')" :class="{active: difficulty === 'impossible'}" class='difficulty__btn'>Impossible</button>
          </td>
        </tr>
      </tbody>
    </table>

</template>

<script>

export default {
  name: "Difficulty",
  components: {
  },
  props: {
    difficulty: String,
  },
  data() {
    return {
      content: ""
    }
  },
  created() {
  },
  methods: {
    onChangeDifficulty(lvl) {
      this.$emit('change-difficulty', lvl);
    },
  }
};
</script>

<style lang="scss" scoped>
  .difficulty {
    font-size: 1rem;
    margin: auto;
    margin-bottom: 4rem;

    td {
      width: 80px;
    }
    &__btn {
      opacity: .5;
      cursor: pointer;
      background: transparent;
      border: none;
      font-family: var(--font);
      color: var(--body);
      font-size: 1rem;

      &.active {
        cursor: auto;
        opacity: 1;
        animation: blink .8s;
      }
    }
  }

  @keyframes blink {
  0% { opacity: .5; }
  10% { opacity: 1; }
  20% { opacity: .5; }
  30% { opacity: 1; }
  50% { opacity: .5; }
  100% { opacity: 1; }
}
</style>

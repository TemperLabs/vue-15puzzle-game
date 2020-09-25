
<template>
  <div class="box">
      <transition-group name="shuffle" tag="ul"
                        class="puzzle-wrap"
                        :style="{ width: metrics, height: metrics }">
      <li
        :class="{'puzzle': true, 'puzzle-empty': puzzle === ''}"
        v-for="(puzzle, index) in puzzleComp"
        :key="puzzle"
        v-text="puzzle"
        @click="swap(index)"
      ></li>
      </transition-group>
    <h2>Сложность:</h2>
    <div>
      <select :value="depth" @input="reRender">
      <option disabled value="">Выберите один из вариантов</option>
      <option :value="3">9</option>
      <option :value="4">16</option>
      <option :value="5">25</option>
    </select>
    </div>
    <div>
      <button type="button" @click="render">
        reset
      </button>
    </div>
    <div>
      <button type="button" @click="shuffle">
        shuffle
      </button>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      depth: 3,
      puzzles: []
    }
  },
  computed: {
    metrics () {
      return (this.depth * 100) + 'px'
    },
    puzzleComp () {
      return this.puzzles
    }
  },
  methods: {
    reRender (event) {
      console.log(event.target.value)
      this.depth = event.target.value
      this.render()
    },
    render () {
      let puzzleArr = []
      for (let i = 1; i < this.depth * this.depth; i++) {
        puzzleArr.push(i.toString())
      }
      puzzleArr.push('')
      puzzleArr = puzzleArr.sort(() => {
        return Math.random() - 0.5
      })

      this.puzzles = puzzleArr
    },
    shuffle () {
      this.puzzles = this.puzzles.sort(() => {
        return Math.random() - 0.5
      })
    },
    swap (index) {
      const swipeArr = [1, -1, +this.depth, -this.depth]
      console.log(swipeArr)
      const originIndex = swipeArr.find((swipeIndex) => this.puzzles[swipeIndex + index] === '')
      if (originIndex) {
        const origin = this.puzzles[index]
        this.puzzles.splice(index, 1, this.puzzles[originIndex + index])
        this.$set(this.puzzles, originIndex + index, origin)
      }
    }
  },
  created () {
    this.render()
  }
}

</script>
<style lang="scss">
  ul,li,ol {
    list-style: none;
    padding: 0;
    margin: 0;
  }

  .box {
    width: 400px;
    margin: 50px auto 0;
  }

  .puzzle-wrap {
    display: flex;
    flex-wrap: wrap;
    margin-bottom: 40px;
    padding: 0;
    background: #ccc;
    list-style: none;
  }

  .puzzle {
    width: 100px;
    height: 100px;
    font-size: 20px;
    background: #62ffb2;
    text-align: center;
    line-height: 100px;
    border: 1px solid #9cc2ff;
    box-shadow: 1px 1px 4px;
    text-shadow: 1px 1px 2px #B9B4B4;
    cursor: pointer;
  }

  .puzzle-empty {
    background: #a96ecc;
    box-shadow: inset 2px 2px 18px;
  }

  .btn-reset {
    box-shadow: inset 3px 3px 18px;
  }

  .shuffle-move {
    transition: transform 1s;
  }

  .shuffle-enter-active, .shuffle-leave-active {
    transition: all 1s;
  }
  .shuffle-enter, .shuffle-leave-to /* .list-leave-active до версии 2.1.8 */ {
    opacity: 0.5;
    transform: translateY(30px);
  }

</style>

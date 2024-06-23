<template>
  <div id="game-container">
    <div id="computer" :style="computedStyleObject"></div>
    <div class="buttons">
      <button @click="onClickButton('Rock')">Rock</button>
      <button @click="onClickButton('Scissors')">Scissors</button>
      <button @click="onClickButton('Paper')">Paper</button>
    </div>
    <div class="result">{{ result }}</div>
    <div class="score">Current score: {{ score }}.</div>
  </div>
</template>

<script>
const rspCoords = {
  Rock: '0',
  Scissors: '-142px',
  Paper: '-284px'
}

const scores = {
  Scissors: 1,
  Rock: 0,
  Paper: -1
}

const computerChoice = (imgCoord) => {
  return Object.entries(rspCoords).find(function (v) {
    return v[1] === imgCoord
  })[0]
}

export default {
  data() {
    return {
      imgCoord: rspCoords.Rock,
      result: '',
      score: 0,
      interval: null
    }
  },
  computed: {
    computedStyleObject() {
      return {
        background: `url(https://en.pimg.jp/023/182/267/1/23182267.jpg) ${this.imgCoord} 0`
      }
    }
  },
  methods: {
    changeHand() {
      this.interval = setInterval(() => {
        if (this.imgCoord === rspCoords.Rock) {
          this.imgCoord = rspCoords.Scissors
        } else if (this.imgCoord === rspCoords.Scissors) {
          this.imgCoord = rspCoords.Paper
        } else if (this.imgCoord === rspCoords.Paper) {
          this.imgCoord = rspCoords.Rock
        }
      }, 100)
    },
    onClickButton(choice) {
      clearInterval(this.interval)
      const myScore = scores[choice]
      const cpuScore = scores[computerChoice(this.imgCoord)]
      const diff = myScore - cpuScore
      if (diff === 0) {
        this.result = 'This is a draw.'
      } else if ([-1, 2].includes(diff)) {
        this.result = 'You won.'
        this.score += 1
      } else {
        this.result = 'You lost.'
        this.score -= 1
      }
      setTimeout(() => {
        this.changeHand()
      }, 1000)
    }
  },
  mounted() {
    console.log('mounted')
    this.changeHand()
  },
  beforeUnmount() {
    clearInterval(this.interval)
  }
}
</script>

<style scoped>
#game-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
}

#computer {
  width: 142px;
  height: 200px;
  background-position: 0 0;
}

.buttons {
  margin-top: 20px;
}

.buttons button {
  margin: 5px;
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
}

.result {
  margin-top: 20px;
  font-size: 18px;
  font-weight: bold;
}

.score {
  margin-top: 10px;
  font-size: 16px;
}
</style>

<template>
    <div class="game">
      <div class="game_colors">
        <button class="game_colors__green" v-on:click="pressingColors(0)" :class="{lit: lit == 'green'}"></button>
        <button class="game_colors__red" v-on:click="pressingColors(1)" :class="{lit: lit == 'red'}"></button>
        <button class="game_colors__yellow" v-on:click="pressingColors(2)" :class="{lit: lit == 'yellow'}"></button>
        <button class="game_colors__blue" v-on:click="pressingColors(3)" :class="{lit: lit == 'blue'}"></button>
      </div>
      
      <div class="game_options">
        <button class="game_options__buttonStart" v-on:click="start()">Старт</button>

        <p class="game_options__score">Очки: {{ score }}</p>

        <h3 class="game_options__header">Сложности:</h3>

        <div class="game_options__difficultyChoice">
          <input class="game_options__difficultyChoice__input" type="radio" id="easy" name="difficulty" value="0" v-model="difficulty" />
          <label class="game_options__difficultyChoice__label" for="easy">Легкая</label>
        </div>

        <div class="game_options__difficultyChoice">
          <input class="game_options__difficultyChoice__input" type="radio" id="normal" name="difficulty" value="1" checked v-model="difficulty"/>
          <label class="game_options__difficultyChoice__label" for="normal">Нормальная</label>
        </div>

        <div class="game_options__difficultyChoice">
          <input class="game_options__difficultyChoice__input" type="radio" id="hard" name="difficulty" value="2" v-model="difficulty"/>
          <label class="game_options__difficultyChoice__label" for="hard">Тяжелая</label>
        </div>

        <h1 class="game_options__result" v-if="result">Поражение!</h1>
      </div>
    </div>
</template>

<script>
export default {
  name: 'SimonGame',

  data() {
    return {
      difficulty: '1',
      pressedColors: [],
      subsequence: [],
      lit: '',
      colors: ['green', 'red', 'yellow', 'blue'],
      difficulties: [1500, 1000, 400],
      score: 0,
      result: false,
      audio: new Audio("https://s3.amazonaws.com/freecodecamp/simonSound1.mp3"),
    };
  },

  methods: {
    start() {
      this.subsequence = []
      this.pressedColors = []
      this.score = 0
      this.result = false

      this.litSubsequence()
    },

    litSubsequence() {
      this.subsequence.push(Math.floor(Math.random() * 4))

      for (let i = 0; i < this.subsequence.length; i++) {
        setTimeout(() => {
          this.lit = this.colors[this.subsequence[i]]

          setTimeout(() => {
            this.lit = ''
          }, this.difficulties[this.difficulty] - 200)

        }, this.difficulties[this.difficulty] + (this.difficulties[this.difficulty] * i))
      }
      this.pressedColors = []
    },

    pressingColors(color) {
      this.pressedColors.push(color)
      this.audio.play()

      if ((this.pressedColors[this.pressedColors.length - 1] == this.subsequence[this.pressedColors.length - 1]) && (this.pressedColors.length == this.subsequence.length)) {
        this.pressedColors = []
        this.score++
        this.litSubsequence()
      }

      if ((this.pressedColors[this.pressedColors.length - 1] != this.subsequence[this.pressedColors.length - 1]) && (this.subsequence.length != 0)) {
        this.result = true
      }
    },
  }
};
</script>

<style lang="sass" scoped>
.lit
  filter: brightness(4)

.game
  font-family: sans-serif
  margin-top: 50px
  padding: 10px
  border-radius: 20px
  display: flex
  gap: 50px
  box-shadow: 0px 0px 43px 3px rgba(0, 0, 0, 0.31)
  width: 600px

  &_options
    position: relative
    &__buttonStart
      margin-bottom: 20px
      background-color: blue
      border: none
      border-radius: 5px
      width: 100px
      height: 70px
      color: white
      font-size: 30px

      &:hover
        cursor: pointer
        filter: brightness(0.8)

      &:active
        filter: brightness(4)

    &__difficultyChoice
      padding-top: 10px

    &__score
      font-size: 30px
      padding-bottom: 20px

    &__result
      position: absolute
      top: 300px
      left: -38px
      color: red

  &_colors
    display: grid
    grid-template-columns: repeat(2, 1fr)

    &__green
      width: 200px
      height: 200px
      border: none
      border-top-left-radius: 30px
      background-color: #008311
      
      &:hover
        cursor: pointer
        filter: brightness(1.5)
    
      &:active
        filter: brightness(4)

    &__red
      width: 200px
      height: 200px
      border: none
      border-top-right-radius: 30px
      background-color: #830000
      
      &:hover
        cursor: pointer
        filter: brightness(1.5)
    
      &:active
        filter: brightness(4)

    &__yellow
      width: 200px
      height: 200px
      border: none
      border-bottom-left-radius: 30px
      background-color: #838100
      
      &:hover
        cursor: pointer
        filter: brightness(1.5)
    
      &:active
        filter: brightness(4)

    &__blue
      width: 200px
      height: 200px
      border: none
      border-bottom-right-radius: 30px
      background-color: #001F83
      
      &:hover
        cursor: pointer
        filter: brightness(1.5)

      &:active
        filter: brightness(4)
</style>
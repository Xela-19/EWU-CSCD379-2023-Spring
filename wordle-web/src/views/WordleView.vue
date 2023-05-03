<template>
  <!-- Dialog to get user's name -->
  <UserInfoDialog />
  <v-container>
    <h1 id="title" class="text-center pb-8">Wordle Mind Bender</h1>

    <GameBoard :game="game" @letterClick="addChar" />

    <KeyBoard
      :game="game"
      :guessedLetters="game.guessedLetters"
      @letter-click="addChar($event)"
      @backspace="backspace"
      @check-guess="checkGuess"
    />
    <AvailableWordsButton :wordle-game="game" :key="game.guesses.length" @guessChanged="setGuess" />
    <h3>{{ game.status }}</h3>

    <!-- Results dialog -->
    <v-dialog v-model="showResults" width="500px">
      <v-card class="text-center" height="350px">
        <v-card-title class="text-h5"> Results </v-card-title>
        <v-card-text>
          {{ `You ${game.status === WordleGameStatus.Won ? 'Won!' : 'Lost'}` }}
        </v-card-text>
        <v-card-actions>
          <v-spacer />
          <v-btn variant="tonal" elevation="5" @click="resetGame()"> Play Again </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script setup lang="ts">
import { WordleGame, WordleGameStatus } from '@/scripts/wordleGame'
import { ref, reactive, onMounted, onUnmounted } from 'vue'
import GameBoard from '../components/GameBoard.vue'
import KeyBoard from '../components/KeyBoard.vue'
import { Letter } from '@/scripts/letter'
import AvailableWordsButton from '@/components/AvailableWordsButton.vue'
import UserInfoDialog from '@/components/UserInfoDialog.vue'

const guess = ref('')
const game = reactive(new WordleGame())

onMounted(async () => {
  window.addEventListener('keyup', keyPress)
  await game.restartGame()
  console.log(game.secretWord)
})
onUnmounted(() => {
  window.removeEventListener('keyup', keyPress)
})
let showResults = false

function checkGuess() {
  if (guess.value.length < game.secretWord.length) return
  game.submitGuess()
  if (game.status !== WordleGameStatus.Active) {
    showResults = true
  }
  guess.value = ''
}

function addChar(letter: Letter) {
  if (guess.value.length >= game.secretWord.length) return
  if (!/[a-zA-Z]/.test(letter.char)) return
  game.guess.push(letter.char)
  guess.value += letter.char
}

function backspace() {
  game.guess.pop()
  guess.value = guess.value.slice(0, -1)
}

function setGuess(value: string) {
  game.guess.clear()
  for (let i = 0; i < value.length; i++) {
    addChar(new Letter(value[i]))
  }
}

function resetGame() {
  game.restartGame()
  showResults = false
}

function keyPress(event: KeyboardEvent) {
  console.log(event.key)
  if (event.key === 'Enter') {
    checkGuess()
  } else if (event.key === 'Backspace') {
    guess.value = guess.value.slice(0, -1)
    game.guess.pop()
    console.log('Back')
  } else if (event.key.length === 1 && event.key !== ' ') {
    guess.value += event.key.toLowerCase()
    game.guess.push(event.key.toLowerCase())
  }
}
</script>

<style>
#title {
  font-size: 50px;
  font-family: fantasy;
}
</style>

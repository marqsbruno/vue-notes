<script setup>
import { ref } from 'vue'
const showModal = ref(false)
const newNote = ref('')
const cards = ref([])
const errorMsg = ref(false)

function addNewCard() {
  if (newNote.value.length < 3) {
    errorMsg.value = true
    return
  }

  function getUniqueId() {
    return `${Date.now()}-${Math.floor(Math.random() * 10000)}`
  }

  function getRandomColor() {
    const orange = '#FF7800'
    const blue = '#6BB2F1'
    const yellow = '#FEF65C'
    const darkYellow = '#FEF65C'
    const purple = '#9C94E8'
    const pink = '#FC8CAB'
    const turquoise = '#36D3ED'

    const random = Math.floor(Math.random() * 7)

    switch (random) {
      case 0:
        return orange
      case 1:
        return blue
      case 2:
        return yellow
      case 3:
        return darkYellow
      case 4:
        return purple
      case 5:
        return pink
      case 6:
        return turquoise
      default:
        return orange
    }
  }

  cards.value.push({
    id: getUniqueId(),
    text: newNote.value,
    date: new Date(),
    backgroundColor: getRandomColor()
  })
  showModal.value = false
  newNote.value = ''
  errorMsg.value = false
}

function handleDelete(id) {
  cards.value = cards.value.filter((item) => item.id !== id)
  console.log(cards.value)
}
</script>

<template>
  <main>
    <div class="overlay" v-if="showModal">
      <div class="modal">
        <textarea
          v-model.trim="newNote"
          name="notes"
          id="notes"
          cols="5"
          maxlength="150"
          rows="7"
        ></textarea>
        <p v-if="errorMsg">Need 3 characters or more</p>
        <button v-on:click="addNewCard">add note</button>
        <button class="closeBtn" @click="showModal = false">close</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal = true">+</button>
      </header>
      <div class="cards-container">
        <div
          class="card"
          v-for="card in cards"
          :style="{ backgroundColor: card.backgroundColor }"
          :key="card.id"
        >
          <button class="deleteCard" @click="handleDelete(card.id)">x</button>
          <div class="card-info">
            <p class="main-text">
              {{ card.text }}
            </p>
            <p class="date">{{ card.date.toLocaleString('pt-BR') }}</p>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  height: 100vh;
  width: 100vw;
  word-break: break-word;
}

.main-text {
  font-size: 22px;
  display: flex;
}

.container {
  max-width: 1000px;
  padding: 10px;
  margin: 0 auto;
}

header {
  display: flex;
  justify-content: space-around;
  align-items: center;
}

h1 {
  font-weight: bold;
  margin-bottom: 25px;
  font-size: 75px;
}

header button {
  border: none;
  padding: 10px;
  cursor: pointer;
  border-radius: 100%;
  width: 40px;
  height: 40px;
  background-color: black;
  color: aliceblue;
  font-weight: bold;
  font-size: large;
}

.card {
  width: 250px;
  height: 250px;
  padding: 15px;
  border-radius: 20px;
  display: flex;
  flex-direction: column;
  align-content: center;
  margin-right: 20px;
  margin-bottom: 20px;
}

.deleteCard {
  width: 13px;
  height: 13px;
  border-radius: 50%;
  border: none;
  background-color: rgb(41, 41, 41);
  color: white;
  font-size: 10px;
  align-self: flex-end;
}

.date {
  color: rgb(40, 68, 68);
  font-size: 12px;
}

.cards-container {
  display: flex;
  flex-wrap: wrap;
}

.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.77);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal {
  width: 750px;
  background-color: white;
  border-radius: 10px;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
}

.modal button {
  padding: 8px;
  font-size: 17px;
  width: 100%;
  cursor: pointer;
  margin-top: 10px;
  align-self: center;
  background-color: blueviolet;
  color: white;
  border: none;
}

.modal .closeBtn {
  background-color: red;
  color: white;
}

.card-info {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  width: 100%;
  height: 100%;
}
</style>

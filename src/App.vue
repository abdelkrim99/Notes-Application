<script setup>

import { ref } from "vue";

const showModal = ref(false);
const newNote = ref();
const errMessage = ref("")
const notes = ref([]);

const addNote = () => {
  if (newNote.value.length < 10) {
    return errMessage.value = "the note must contain at least 10 caracters"
  }
  notes.value.push({
    id: Math.floor(Math.random() * 1000000),
    text: newNote.value,
    date: new Date(),
    color: getRandomColor()
  });
  showModal.value = false;
  newNote.value = "";
  errMessage.value = "";
}

const closeNote = () => {
  newNote.value = ""
  showModal.value = false
  errMessage.value = ""
}

function getRandomColor() {
  return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
}

</script>


<template>
  <body>
    <transition name="overlayAnimation">
      <div v-if="showModal" class="overlay">
        <div class="modal">
          <h2>tap your note ...</h2>
          <textarea v-model="newNote" name="note" id="note" cols="30" rows="10"></textarea>
          <p v-if="errMessage" class="errShow">{{ errMessage }}</p>
          <button @click="addNote()" class="add-button" role="button">Add Note</button>
          <button class="close-button" role="button" @click="closeNote()">close</button>
        </div>
      </div>
    </transition>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button class="plus-button" @click="showModal = true">+</button>
      </header>
      <div class="cards-container">
        <div v-for="note in notes" key="note.key" class="card" :style="{ backgroundColor: note.color }">
          <p class="main-text">{{ note.text }}</p>
          <p class="date"> {{ note.date.toLocaleDateString("fr") }}</p>
        </div>
      </div>
    </div>
      <h1 v-show="notes.length < 1" class="intro">no notes yet</h1>
  </body>
</template>

<style scoped>
body {
  background-color: rgb(250, 250, 250);
}

h2 {
  font-family: 'Brush Script MT', cursive;
  padding-bottom: 5px;
}

.container {
  max-width: 1000px;
  padding: 10px;
  margin: 0 auto;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 50px;
}

.intro {
  position: absolute;
  top: 60%;
  left: 40%;
  color: rgb(235, 235, 235);
  font-family: 'Brush Script MT', cursive;
  top: 40%;
}

h1 {
  font-family: 'Brush Script MT', cursive;
  font-size: 50px;
}

.plus-button {
  background-color: rgb(0, 226, 0);
  width: 50px;
  height: 50px;
  border: 0px;
  border-radius: 100px 100px;
  font-size: 40px;
  cursor: pointer;
  box-shadow: rgba(0, 0, 0, 0.35) 0px 5px 15px;
  color: aliceblue;
}

.plus-button:hover {
  background-color: rgb(0, 214, 0);
}

.plus-button:active {
  box-shadow: rgba(0, 0, 00, 0) 0px 0px 0px;
  transition: .1s;
}

.cards-container {
  display: flex;
  flex-wrap: wrap;
}

/* card style start  */

.card {
  width: 225px;
  height: 235px;
  padding: 5px 8px 2px 8px;
  border-radius: 10px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 15px;
  overflow-wrap: break-word;

  overflow: auto;
  color: rgba(0, 0, 0, 0);
  -webkit-text-fill-color: black;
  box-shadow: 0 1px 6px rgba(0, 0, 0, 0.2);
  transition: color .2s ease;
}

*::-webkit-scrollbar,
*::-webkit-scrollbar-thumb {
  width: 5px;
  border-radius: 10px;
  background-clip: padding-box;
  /* border: 15px solid transparent; */
  background-clip: 20px solid black;
}

*::-webkit-scrollbar-thumb {        
  box-shadow: inset 0 0 0 10px;
}

 .card:hover {
  color: rgba(0, 0, 0, 0.3);
} 

/* card style end  */

.date {
  align-self: flex-end;
  font-weight: bold;
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
  /* transition: opacity .2s ease; */
}

.modal {
  background-color: white;
  width: 500px;
  padding: 15px;
  display: flex;
  flex-direction: column;
  position: relative;
  justify-content: space-between;
  border-radius: 3px;
}

/*.add-button {
  padding: 10px 20px;
  font-size: 20px;
  border-radius: 10px;
  background-color: bisque;
}*/


/* CSS */
.add-button {
  background-color: #13aa52;
  border: 1px solid #13aa52;
  border-radius: 4px;
  box-shadow: rgba(0, 0, 0, .1) 0 2px 4px 0;
  box-sizing: border-box;
  color: #fff;
  cursor: pointer;
  font-family: "Akzidenz Grotesk BQ Medium", -apple-system, BlinkMacSystemFont, sans-serif;
  font-size: 16px;
  font-weight: 400;
  outline: none;
  outline: 0;
  padding: 10px 25px;
  text-align: center;
  transform: translateY(0);
  transition: transform 150ms, box-shadow 150ms;
  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;
  margin-bottom: 10px;
  margin-top: 10px;
}

.add-button:hover {
  box-shadow: rgba(0, 0, 0, .15) 0 3px 9px 0;
  transform: translateY(-2px);
}

@media (min-width: 768px) {
  .add-button {
    padding: 10px 30px;
  }
}

/* CSS */
.close-button {
  background-color: #ee0000;
  border: 1px solid #ee0000;
  border-radius: 4px;
  box-shadow: rgba(0, 0, 0, .1) 0 2px 4px 0;
  box-sizing: border-box;
  color: #fff;
  cursor: pointer;
  font-family: "Akzidenz Grotesk BQ Medium", -apple-system, BlinkMacSystemFont, sans-serif;
  font-size: 16px;
  font-weight: 400;
  outline: none;
  outline: 0;
  padding: 10px 25px;
  text-align: center;
  transform: translateY(0);
  transition: transform 150ms, box-shadow 150ms;
  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;
}

.close-button:hover {
  box-shadow: rgba(0, 0, 0, .15) 0 3px 9px 0;
  transform: translateY(-2px);
}

@media (min-width: 768px) {
  .close-button {
    padding: 10px 30px;
  }
}

textarea {
  resize: none;
}

.errShow {
  color: red;
}

/* overlay animation start */

.overlayAnimation-enter-from {
  opacity: 0
}

.overlayAnimation-enter-to {
  opacity: 1
}

.overlayAnimation-enter-active {
  transition: all .25s ease;
}

.overlayAnimation-leave-from {
  opacity: 1;
}

.overlayAnimation-leave-to {
  opacity: 0;
}

.overlayAnimation-leave-active {
  transition: all .25s ease;
}

/* overlay animation end */




</style>
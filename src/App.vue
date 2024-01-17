<script setup>
  import { ref } from 'vue';

  const showModal = ref(false);
  
  const newNote = ref("");
  const notes = ref([]);

  const errorMessage = ref("");

  function getRandomColor() {
    return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
  }

  const addNote = () => {
    if (newNote.value.length < 10) {
      errorMessage.value = "Your note must be 10 characters or more";
      return;
    } 
    notes.value.push({
      id: Math.floor(Math.random() * 100),
      text: newNote.value,
      date: new Date(),
      backgroundColor: getRandomColor()  
    })
    showModal.value = false;
    newNote.value = "";
    errorMessage.value = "";
  }
</script>

<template>
  <main>
    <div v-if="showModal" class="overlay">
      <div class="modal">
        <textarea v-model.trim="newNote" name="note" id="note" cols="30" rows="10"></textarea>
        <p v-if="errorMessage">{{ errorMessage }}</p>
        <button @click="addNote">Add Note</button>
        <button @click="showModal = false" class="close">Close</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal = true">+</button>
      </header>
      <div class="cards-container">
        <div 
          v-for="note in notes"
          :key="note.id" 
          class="card" 
          :style="{backgroundColor: note.backgroundColor}"
        >
          <p class="main-text">{{  note.text }}</p>
          <p class="date"> {{  note.date.toLocaleDateString("en-US") }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
  main {
    height: 100vh;
    width: 100vw;
  }
  .container {
    width: 1000px;
    padding: 10px;
    margin: 0 auto;
  }

  header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 20px; /* Add some padding on smaller screens */
}

@media (min-width: 768px) {
  header {
    justify-content: space-between;
    padding: 0; /* Remove the padding on larger screens */
  }
}

h1 {
  font-weight: bold;
  font-size: 3em; /* Use em for scalable font size */
  margin-bottom: 25px;
}

@media (min-width: 768px) {
  h1 {
    font-size: 4em; /* Larger font size on larger screens */
  }
}

  header button {
    border: none;
    padding: 10px;
    width: 50px;
    height: 50px;
    cursor: pointer;
    background-color: black;
    color: white;
    border-radius: 100%;
    font-size: 1em; /* Use em for scalable font size */
  }

  @media (min-width: 768px) {
    header button {
      width: 60px; /* Larger button on larger screens */
      height: 60px;
      font-size: 1.2em; /* Larger font size on larger screens */
    }
  }

  .card {
    width: 225px;
    height: 225px;
    padding: 10px;
    background-color: #f8ef71;
    border-radius: 15px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    margin-right: 25px;
    margin-bottom: 25px;
  }
  .date {
    font-size: 12px;
    font-weight: bold;
    color: #a8a8a8;
  }
  .cards-container {
    display: flex;
    flex-wrap: wrap;
  }

  .overlay {
    position: absolute;
    width: 100%;
    height: 100%;
    background-color: rgba(0,0,0,0.5);
    z-index: 10;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .modal {
    width: 700px;
    background-color: white;
    border-radius: 15px;
    padding: 30px;
    position: relative;
    display: flex;
    flex-direction: column;
    padding: 25px;
  }

  .modal button {
    padding: 10px;
    font-size: 20px;
    width: 100%;
    background-color: blueviolet;
    color: white;
    border: none; 
    cursor: pointer;
    border-radius: 15px;
    margin-top: 15px;
  }

  .modal .close {
    background-color: red;
  }

  .modal p {
    color: red;
  }
</style>



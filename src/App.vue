<script setup>
import { ref } from "vue";

const showModal = ref(false);
const showEditModal = ref(false);
const note = ref("");
const editId = ref("");

const notes = ref([]);

function getRandomColor() {
  const color = "hsl(" + Math.random() * 360 + ", 100%, 75%)";
  return color;
}

const reset = () => {
  note.value = "";
  showModal.value = false;
  showEditModal.value = false;
  editId.value = "";
};

const addNotes = () => {
  notes.value.push({
    id: Math.floor(Math.random() * 100000),
    text: note.value,
    color: getRandomColor(),
    date:
      new Date().toLocaleDateString() +
      " at " +
      new Date().toLocaleTimeString(),
  });
  reset();
};

const handleEditNote = (item) => {
  editId.value = item.id;
  showEditModal.value = true;
  note.value = item.text;
};

const editNotes = () => {
  const index = notes.value.findIndex((i) => i.id === parseInt(editId.value));
  if (index !== -1) {
    const editedItem = {
      ...notes.value[index],
      ...{
        text: note.value,
        date:
          "Edited: " +
          new Date().toLocaleDateString() +
          " at " +
          new Date().toLocaleTimeString(),
      },
    };
    notes.value.splice(index, 1, editedItem);
  }
  reset();
};
</script>

<template>
  <main>
    <div v-show="showModal" class="overlay">
      <div class="modal">
        <p @click="reset()">&times;</p>
        <textarea v-model="note" />
        <button @click="addNotes">Add Notes</button>
      </div>
    </div>
    <div v-show="showEditModal" class="overlay">
      <div class="modal">
        <p @click="reset()">&times;</p>
        <textarea v-model="note" />
        <button @click="editNotes">Edit Notes</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal = true">+</button>
      </header>
      <div class="cards-container">
        <div
          v-for="item in notes"
          :key="item.id"
          @click="handleEditNote(item)"
          class="card"
          :style="{ backgroundColor: item.color }"
        >
          <p class="main-text">{{ item.text }}</p>
          <span class="date">{{ item.date }}</span>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
.container {
  max-width: 1000px;
  padding: 10px;
  margin: 0 auto;
}

h1 {
  font-weight: bold;
  margin-bottom: 25px;
  font-size: 75px;
}

.card {
  width: 225px;
  height: 225px;
  background-color: rgb(237, 182, 44);
  padding: 10px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  cursor: pointer;
  margin-bottom: 20px;
}

.main-text {
  line-height: 1.25;
  font-size: 14px;
}

.date {
  font-size: 12.5px;
  margin-top: auto;
  font-weight: bold;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

header button {
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  background-color: rgb(21, 20, 20);
  border-radius: 1000px;
  color: white;
  font-size: 20px;
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
  transform: translate(-50%, -50%);
  top: 50%;
  left: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 10;
}

main {
  height: 100vh;
  width: 100vw;
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
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  background-color: blueviolet;
  border: none;
  color: white;
  cursor: pointer;
  margin-top: 15px;
}

.modal p {
  margin-left: auto;
  font-size: 20px;
  z-index: 100000;
  cursor: pointer;
}

textarea {
  width: 100%;
  height: 200px;
  padding: 5px;
  font-size: 20px;
}
</style>

<script setup>
import { ref } from "vue";
const showModal = ref(false);
const newNote = ref("");
const errorMessage = ref("");
const notes = ref([]);
function getRandomColor() {
  return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
}
const addNote = () => {
  if (newNote.value.length <= 10) {
    errorMessage.value = "You have to write at least more tah 10 characters";

    setTimeout(() => {
      errorMessage.value = "";
    }, 2000);
    return;
  }
  notes.value.push({
    id: Math.floor(Math.random() * 1000000),
    text: newNote.value,
    date: new Date(),
    bacgroundColor: getRandomColor(),
  });
  showModal.value = false;
  newNote.value = "";
};
</script>

<template>
  <main class="h-screen w-screen">
    <div
      v-if="showModal"
      class="overlay absolute w-full h-full bg-slate-600 bg-opacity-50 z-10 flex items-center justify-center"
    >
      <div
        class="modal max-w-3xl w-full bg-gray-300 rounded-xl p-7 relative flex flex-col"
      >
        <textarea
          v-model.trim="newNote"
          class="p-3"
          name="note"
          id="note"
          cols="30"
          rows="10"
          placeholder="Type your note"
        ></textarea>
        {{ errorMessage }}
        <button
          @click="addNote"
          class="py-3 px-5 text-xl w-full bg-violet-700 border-none text-white cursor-pointer mt-4 rounded-3xl"
        >
          Add Note
        </button>
        <button
          @click="showModal = false"
          class="py-3 px-5 text-xl w-full bg-red-500 mt-2 border-none text-white cursor-pointer rounded-3xl"
        >
          Close
        </button>
      </div>
    </div>
    <div class="max-w-screen-xl mx-auto p-3">
      <header class="flex justify-between items-center">
        <h1 class="font-bold mb-6 text-7xl">Notes</h1>
        <button
          @click="showModal = true"
          class="w-12 h-12 cursor-pointer bg-black rounded-full text-white text-xl flex justify-center items-center font-bold"
        >
          +
        </button>
      </header>
      <div class="cardWrap flex flex-wrap">
        <div
          v-for="note in notes"
          :key="note.id"
          :style="{ backgroundColor: note.bacgroundColor }"
          class="card box-border w-56 h-56 bg-purple-500 rounded-2xl flex flex-col justify-between p-3 mr-5 mb-5"
        >
          <p class="main break-words">
            {{ note.text }}
          </p>
          <p class="date text-xs">
            {{ note.date.toLocaleDateString("en-US") }}
          </p>
        </div>
      </div>
    </div>
  </main>
</template>

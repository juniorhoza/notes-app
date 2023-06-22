<script setup>
import {ref} from "vue";

const showModal = ref(false)
const newNote= ref("");
const notes = ref([]);

window.onload=(e)=>{
  notes.value = JSON.parse(localStorage.getItem("notes")) || []
}

function getRandomColor(){
  return  "hsl("+Math.random()*360+ ", 100%,75%)";

}
const addNote = ()=>{
  console.log(notes)
  if(newNote.value.length>10){
    notes.value.push({
    id: Math.floor(Math.random * 10000000),
    text: newNote.value,
    date: new Date().toDateString("en-US"),
    color: getRandomColor()
  })
  newNote.value = "";
  showModal.value = false;

  }
  else{
    alert("notes should be atleast 10 characters long")
  }
 
 
  
}
const removeCard=(index)=>{
  notes.value.splice(index,1)
  }
window.onbeforeunload=(e)=>{
  localStorage.setItem("notes",JSON.stringify(notes.value))
}
</script>
<template >
  <main>
    <div v-if="showModal" class="overlay" >
      <div class="modal">
        <button @click="showModal=false" class="close">
          x
        </button>
        <textarea v-model="newNote" name="notes" id="notes" ></textarea>
        <button @click="addNote" class="submit">
          Add Notes
        </button>
        
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal=true">+</button>
      </header>
      <div class="cards_container" >
        <div class="card" 
        v-for="note,index in notes" 
        :key="note.id" 
        :style="{backgroundColor:note.color}"
        @dblclick="removeCard(index)"
        >
        
          <p class="main_text">
              {{ note.text }}
          </p>
          <p class="date">
            {{ note.date }}
          </p>
        </div>
       
      </div>
    </div>

  </main>
</template>
<style scoped>
main{
  height: 100vh;
  width: 100vw;
}
.container{
 max-width: 1000px;
 padding: 10px;
 margin: 0 auto;
}
header{
  display: flex;
  justify-content: space-between;
  align-items: center;
}
h1{
  font-weight: bold;
  margin-bottom: 25px;
  font-size: 70px;
}
header>button{
  border: none;
  background-color: black;
  color: white;
  width: 50px;
  height: 50px;
  padding: 10px;
  box-shadow: 0;
  border-radius: 100%;
  font-size: 20px;
}
.card{
  width: 225px;
  height: 225px;
  background-color: rgb(237, 182, 44);
  border-radius: 20px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-bottom: 20px;
  margin-right: 15px;
  padding: 10px;
}
.cards_container{
  display: flex;
  flex-wrap: wrap;
  flex-direction: row;
}
.overlay{
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.78);
  z-index: 999;
  display: flex;
  justify-content: center;
  align-items: center;
}
.modal{
  width: 750px;
  background-color: white;
  border-radius: 10px;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
  gap: 20px;
}
textarea{
  resize: none;
  width: 700px;
  height: 200px;
  margin: auto;
}
.modal button{
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  background-color: blueviolet;
  color: white;
  cursor: pointer;
  border: none;
  border-radius: 10px;
  }
  .modal .close{
    background-color: rgb(180, 11, 11);
    width: 35px;
    height: 35px;
    text-align: center;
    float: right;
    color: white;
    font-weight: bold;
    border-radius: 100%;
    padding: 10px;
    position: relative;
    left: 700px;
    top: -50px;

}
</style>
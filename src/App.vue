<script setup>
import { reactive } from 'vue';
import Calculator from './components/calculator.vue'
const key = "THEME"


const estado = reactive({
  theme: "light"
})

const changeTheme = () => {
  estado.theme = estado.theme === "dark" ? "light" : "dark"
  saveToWebStorage(estado.theme)
}

if(typeof(Storage) !== "undefined") {

let theme = getFromWebStorage()

if ( theme !== 'light' )
estado.theme = estado.theme === "dark"

} else {
  console.log('Web storage is not supported')
}


function saveToWebStorage(value) {
  localStorage.setItem(key, value)
}

function getFromWebStorage() {
  return localStorage.getItem(key)
}

</script>

<template>
  <div class="container" :class="estado.theme === 'light' ? 'container--light' : 'container--dark'">
    <button class="btn_change_theme" @click="changeTheme" type="button">
      <img v-if="estado.theme  === 'light'" src="./assets/img/icons/dark/icons8-símbolo-da-lua-32.png" alt="">
      <img v-else src="./assets/img/icons/icons8-luz-acesa-32.png" alt="">
    </button>
    <div :class="estado.theme === 'light' ?  'calculator__container' : 'calculator__container dark'">
      <Calculator :theme="estado.theme"/>
    </div>
  </div>
</template>

<style scoped>
  input { 
    margin-bottom: 16px;
  }
  
  .btn_change_theme {
    position: absolute;
    top: 20px;
    right: 20px;
    border: none;
    background-color: transparent;
    cursor: pointer;
  }
  .btn_change_theme:hover {
    transform: scale(1.2);
    transition: all ease 0.3s;
  } 
  .calculator__container {
    max-width: 400px;
    width: 100%;
    padding: 8px;
    background-color: #45547c;
  }
  .dark {
    background-color: rgb(163, 163, 163);
  }
  .container.container--light{
    background-color: rgb(224, 236, 253);
  }
  .container.container--dark{
    background-color: rgb(37, 4, 15);
  }

  @media screen and (max-width: 480px) {
    .calculator__container {
      height: 100vh;
      max-width: 100%;
      width: 100%;
      padding-top: 30%;
    }
  }
  @media screen and (min-width: 481px) and (max-width: 768px) {
    .calculator__container {
      max-width: 540px;
      height: auto;
    }
  }
</style>

<script setup>
import axios from "axios";
import { ref, onMounted, computed } from 'vue'

const pokemon = ref(null);

const pokemonName = computed(()=>{
  if(pokemon && pokemon.value !== null && pokemon.value.name){
      let firstLetter = pokemon.value.name[0].toUpperCase();
      return firstLetter + pokemon.value.name.substring(1);
  }else{
      return "N/A"
  }
})

const getPokemon = async () => {
    const randomId = Math.floor(Math.random()*806 + 1);
    try {
        const response = await axios.get(`https://pokeapi.co/api/v2/pokemon/${randomId}/`);
        pokemon.value = response.data;
    } catch (error) {
        console.log("Could'nt get a pokemon: ", error);
    }
}
const getColorType = (type) => {
    let styleStr = "";
    switch(type){
        case "normal":
        styleStr = `background-color:#ACB890`
        break;
        case "fighting":
        styleStr = `background-color:#C0332E`
        break;
        case "flying":
        styleStr = `background-color: #99A6DD`
        break;
        case "poison":
        styleStr = `background-color: #BB5CA1`
        break;
        case "ground":
        styleStr = `background-color: #E9D4A8`
        break;
        case "rock":
        styleStr = `background-color: #B9A463`
        break;
        case "bug":
        styleStr = `background-color: #A4B748`
        break;
        case "ghost":
        styleStr = `background-color: #6362AC`
        break;
        case "steel":
        styleStr = `background-color: #B0AEC5`
        break;
        case "fire":
        styleStr = `background-color: #FB4A3B`
        break;
        case "water":
        styleStr = `background-color: #2A94F0`
        break;
        case "grass":
        styleStr = `background-color: #75D76E`
        break;
        case "electric":
        styleStr = `background-color: #F8C453`
        break;
        case "psychic":
        styleStr = `background-color: #F66F9D`
        break;
        case "ice":
        styleStr = `background-color: #4DCDE1`
        break;
        case "dragon":
        styleStr = `background-color: #856BEC`
        break;
        case "dark":
        styleStr = `background-color: #735A4C`
        break;
        case "fairy":
        styleStr = `background-color: #F9B7ED`
        break;
        case "unknown":
        styleStr = `background-color: #BBBBBB`
        break;
        case "shadow":
        styleStr = `background-color: #5f5f5f; color: #gggggg;`
        break;
    }
    return styleStr;
  }

onMounted(() => {
  getPokemon();
})
</script>

<template>
  <div class="window poke__container">
      <div class="title-bar">
        <h1 class="title">{{ pokemonName }}</h1>
      </div>
      <div class="separator"></div>
      <div class="window-pane">
        <div class="poke__data__container">
          <div class="poke__data__sprite">
            <img v-if="pokemon && pokemon !== null" :src="pokemon.sprites.front_default" :alt="pokemon.name">
            <p v-else>Waiting for data</p>
          </div>
          <div v-if="pokemon && pokemon !== null" class="poke__data">
            <p>ID: <span>{{pokemon.id}}</span></p>
            <p>Height: <span>{{pokemon.height}}</span></p>
            <p>Weight: <span>{{pokemon.weight}}</span></p>
            <ul class="poke__data__types">
              <li v-for="(slot, index) in pokemon.types " :key="index" :style="getColorType(slot.type.name)"> 
                {{ slot.type.name }} 
              </li>
            </ul>
          </div>
          <div v-else>
            <h5>Waiting for the pokemon's data</h5>
          </div>
        </div>
        <button class="poke__btn btn" @click="getPokemon">
          Get a new pokemon
        </button>
      </div>
    </div>
</template>

<style scoped>
.poke__container{
  color: black;
  width: 30%;
  margin-inline: auto;
}
.poke__data__container{
    width: 100%;
    display: flex;
    align-items: center;
    flex-direction: column;
}
.poke__data__sprite{
    display: flex;
    justify-content: center;
    width: 100%;
}
.poke__data__sprite img{
  width: 60%;
}
.poke__data__types{
  list-style-type: none;
  padding: 0;
}
.poke__data__types li{
  display: inline;
  text-transform: capitalize;
  border-radius: 30px;
  background-color: brown;
  padding: .25rem .75rem .25rem .75rem;
}
.poke__btn{
  margin-top: .75rem;
  display: block;
  margin-inline: auto;
}
.poke__btn:hover{
  background-color: rgba(135, 237, 255, 0.3);
  transition: all .5s ease;
}
</style>

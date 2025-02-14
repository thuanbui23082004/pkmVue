<script setup>
import { ref } from "vue";

const app = ref(null);
const button = ref(null);
const input = ref("");
const start = ref(0);
const limit = ref(18);
const pokemons = ref([]);
const filteredPokemons = ref([]);

const typeColors = {
  grass: "#78cd54",
  poison: "#a33ea1",
  fire: "#ff421c",
  flying: "#a98ff3",
  water: "#6390f0",
  bug: "#a6b91a",
  normal: "#a8a77a",
  electric: "#f7d02c",
  ground: "#e2bf65",
  fairy: "#d685ad",
  fighting: "#c22e28",
  psychic: "#f95587",
  rock: "#b6a136",
  ghost: "#735797",
  ice: "#96d9d6",
  dragon: "#6f35fc",
  dark: "#705746",
  steel: "#b7b7ce",
};

async function cFetch(URL) {
  try {
    const response = await fetch(URL);
    return await response.json();
  } catch (error) {
    console.log("error", error);
    return null;
  }
}

function createPokemonType(types) {
  return types
    .map((type) => {
      const color = typeColors[type.type.name] || "#ccc";
      return `<div class="${type.type.name}" style="background-color: ${color}">${type.type.name}</div>`;
    })
    .join("");
}

async function render() {
  if (!button.value) return;
  button.value.style.display = "block";

  const pokePromises = [];
  for (; start.value < limit.value; start.value++) {
    const pokemon = filteredPokemons.value[start.value];
    if (!pokemon) {
      button.value.style.display = "none";
      break;
    }
    pokePromises.push(cFetch(pokemon.url));
  }

  const pokeData = await Promise.all(pokePromises);
  pokeData.forEach((pokemon) => {
    if (pokemon) {
      app.value.innerHTML += `
        <div class="pokemon">
          <div class="id">#${pokemon.id}</div>
          <img src="https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${
            pokemon.id
          }.png" alt="${pokemon.name}">
          <div class="name">${pokemon.name}</div>
          <div class="type">${createPokemonType(pokemon.types)}</div>
        </div>
      `;
    }
  });

  limit.value += 18;
}

async function fetchPokemons() {
  const data = await cFetch("https://pokeapi.co/api/v2/pokemon/?offset=0&limit=898");
  if (data) {
    pokemons.value = data.results;
    filteredPokemons.value = data.results;
    app.value.innerHTML = "";
    render();
  }
}

function handleSearch() {
  filteredPokemons.value = pokemons.value.filter((pokemon) =>
    pokemon.name.includes(input.value.toLowerCase())
  );

  app.value.innerHTML = "";
  start.value = 0;
  limit.value = 18;
  render();
}
fetchPokemons();
</script>

<template>
  <div class="container">
    <div class="header">
      <div class="heading"><h2>Pokemon API</h2></div>
      <div class="search__bar">
        <input
          v-model="input"
          class="search"
          type="text"
          placeholder="Enter your keyword here..."
          style="width: 100%"
          @input="handleSearch"
        />
      </div>
    </div>

    <div ref="app" class="app">LOADING DATA FROM POKEDEX</div>

    <button ref="button" class="button" @click="render">Load More</button>
  </div>
</template>

<style>
body {
  background-color: #fff;
}
.container {
  font-family: Arial, Helvetica, sans-serif;
  margin-block: 50px;
  font-size: 15px;
  color: #2c3e50;
}
.header {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.heading {
  margin-bottom: 50px;
  font-size: 25px;
}
.heading > h2 {
  font-weight: 400;
}
.search__bar {
  display: flex;
  justify-content: center;
  max-width: 500px;
  width: 100%;
  margin: 0 15px 50px;
}
.search {
  width: 100%;
  padding: 20px;
  border-radius: 30px;
  outline: 1px solid #00000036;
  box-shadow: #64646f33 0 7px 29px;
  font-size: 16px;
  border: none;
  transition: all 0.2s ease;
}
.app {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 10px;
}
.app > * {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 15%;
  padding: 10px 5px;
  cursor: pointer;
  border-radius: 10px;
  box-shadow: #f4f4f4 0 7px 29px;
  text-transform: capitalize;
  transition: 0.3s;
}
.app > *:hover {
  box-shadow: #39393f33 0 7px 29px;
  transition: 0.3s;
}
img {
  width: 100%;
}
.name {
  text-align: center;

  box-shadow: none;
  font-weight: 700;
  font-size: 17px;
}
.type {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  gap: 10px;
  font-size: 13px;
}
.type > * {
  width: 100%;
  padding: 3px 10px;
  border-radius: 7px;
}

.button {
  margin-inline: auto;
  font-size: 16px;
  color: #fff;
  margin-top: 50px;
  cursor: pointer;
  padding: 20px 25px;
  border-radius: 10px;
  background-color: #ff4d4f;
}
</style>

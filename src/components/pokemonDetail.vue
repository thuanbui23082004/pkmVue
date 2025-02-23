<script setup>
import PokemonEvolutionChain from "./PokemonEvolutionChain.vue";

defineProps(["pokemon", "pokemon_desc"]);
defineEmits(["back"]);

function getStatSymbol(statName) {
  const statSymbols = {
    hp: "HP",
    attack: "ATK",
    defense: "DEF",
    "special-attack": "SPA",
    "special-defense": "SPD",
    speed: "SPD",
  };
  return statSymbols[statName];
}
</script>

<template>
  <button @click="$emit('back')" class="btn">&larr; Back</button>
  <div class="pkm__detail">
    <!-- <div class="item__id">#{{ pokemon.id }}</div> -->
    <div
      class="item__image--detail"
      :style="`background-image: url('https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${pokemon.id}.png');`"
    ></div>

    <div class="type">
      <span
        v-for="type in pokemon.types"
        :key="type.type.name"
        class="type__name"
        :class="type.type.name"
      >
        {{ type.type.name }}
      </span>
    </div>
    <div class="item__name--detail item__name">{{ pokemon.name }}</div>
    <p class="pokedex-entry">{{ pokemon_desc }}</p>
    <div class="info">
      <div class="height">
        <div class="text">Height</div>
        <p>{{ pokemon.height }}</p>
      </div>
      <div class="weight">
        <div class="text">Weight</div>
        <p>{{ pokemon.weight }}</p>
      </div>
    </div>

    <div class="abilities">
      <div class="abilities__name">Abilities</div>
      <div>
        <div v-for="ability in pokemon.abilities" class="ability" :key="ability.ability.name">
          {{ ability.ability.name }}
        </div>
      </div>
    </div>
    <div class="stats">
      <div class="stat__title">Stats</div>
      <div class="stat__list">
        <div v-for="stat in pokemon.stats" :key="stat.stat.name" class="stat__name">
          <div class="stat__label" :class="stat.stat.name">
            {{ getStatSymbol(stat.stat.name) }}
          </div>
          <div class="stat__number">
            {{ stat.base_stat }}
          </div>
        </div>
      </div>
    </div>
    <PokemonEvolutionChain
      :speciesUrl="`https://pokeapi.co/api/v2/pokemon-species/${pokemon.id}/`"
    />
  </div>
</template>
<style>
.btn {
  display: block;
  position: fixed;
  font-size: 14px;
  top: 5%;
  left: 5%;
  border-radius: 30px;
  padding: 5px 10px;
  background-color: #fff;
  box-shadow: #63636333 0 4px 8px;
  border: none;
}
.pkm__detail {
  padding-top: 50px;
  margin-block: 50px;
  max-width: 500px;
  width: 100%;
  margin: auto;
  text-align: center;
}
.item__image--detail {
  width: 200px;
  height: 200px;
  margin: auto;
  background-size: cover;
}

.item__name--detail {
  margin-top: 10px;
  font-weight: 700;
  text-align: center;
  text-transform: capitalize;
  font-size: 22px;
}
.info {
  display: flex;
  margin-bottom: 15px;
  gap: 80px;
  justify-content: center;
}
.text {
  font-size: 18px;
  font-weight: 600;
  /* margin-bottom: 5px; */
}
.info p,
.ability {
  background-color: #dff4fa73;
  width: 100px;
  padding: 5px;
  border-radius: 20px;
  margin-block: 6px;
}
.abilities > div {
  display: flex;
  justify-content: center;
  gap: 80px;
}
.abilities__name,
.stat__title {
  font-weight: 700;
  text-align: center;
  text-transform: capitalize;
  font-size: 20px;
  margin-top: 10px;
  margin-bottom: 5px;
}
.ability {
  display: inline-block;
  text-transform: capitalize;
}
.stat__list {
  display: flex;
  justify-content: center;
  gap: 40px;
  margin-top: 10px;
}
.stat__name {
  margin: 5px;
  padding: 5px;
  background-color: #f6f8fc;
  border-radius: 30px;
  box-shadow: #63636333 0 2px 8px;
}
.stat__label {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 8px;
  border-radius: 50%;
  width: 25px;
  height: 25px;
  font-size: 10px;
  font-weight: 700;
  color: #fff;
  text-transform: uppercase;
}
.hp {
  background-color: #df2140;
}

.attack {
  background-color: #ff994d;
}

.defense {
  background-color: #eecd3d;
}

.special-attack {
  background-color: #85ddff;
}

.special-defense {
  background-color: #96da83;
}

.speed {
  background-color: #fb94a8;
}
</style>

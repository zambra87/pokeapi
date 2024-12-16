<script>
import axios from "axios"; 
import PokemonCard from "./components/PokemonCard.vue"; 
export default {
  components: {
    PokemonCard, 
  },
  data() {
    return {
      pokemones: [], 
      loading: true, 
      aciertos: 0, // Contador de aciertos
    };
  },
  async mounted() {
    try {
      const url = "https://pokeapi.co/api/v2/pokemon?limit=20"; 
      const response = await axios.get(url);

      // Itera sobre los resultados para obtener los detalles de cada Pokémon
      this.pokemones = await Promise.all(
        response.data.results.map(async (pokemon) => {
          const pokemonData = await axios.get(pokemon.url); // Solicita los detalles
          return {
            name: pokemonData.data.name, // Nombre del Pokémon
            image: pokemonData.data.sprites.front_default, // Imagen del Pokémon
            guessed: false, // Indica si el usuario adivinó el nombre
          };
        })
      );
    } catch (error) {
      console.error("Error al obtener Pokémon:", error); 
    } finally {
      this.loading = false; 
    }
  },
  methods: {
    verificarNombre(pokemon, nombre) {
      if (pokemon.name.toLowerCase() === nombre.toLowerCase()) {
        pokemon.guessed = true; // Marca como adivinado
        this.aciertos++; // Incrementa el contador de aciertos
      } else {
        alert("¡Nombre incorrecto! Intenta de nuevo."); // Muestra el mensaje de error
      }
    },
  },
};
</script>

<template>
  <div>
    <header>
      <div>
        <img class="img-fluid" src="../public/International_Pokémon_logo.svg.png" alt="pokemon" />
        <h1 class="text-center">¿Quién es ese Pokémon?</h1>
        <h3 class="text-center">Aciertos: {{ aciertos }}</h3> <!-- Contador de aciertos -->
      </div>
    </header>

    <main>
      <div v-if="loading" class="loading-message">Cargando Pokémon...</div> 

      <div id="App" v-else>
        <div v-for="(pokemon, i) in pokemones" :key="i" class="pokemon-card">
          <img :src="pokemon.image" :alt="pokemon.name" :style="{ filter: pokemon.guessed ? 'none' : 'blur(5px) grayscale(100%)' }" />
          <div v-if="!pokemon.guessed">
            <input v-model="pokemon.input" placeholder="Ingresa el nombre..." />
            <button @click="verificarNombre(pokemon, pokemon.input)">Descubrir</button>
          </div>
          <p v-if="pokemon.guessed">{{ pokemon.name }}</p> <!-- Muestra el nombre si se adivinó -->
        </div>
      </div>
    </main>
  </div>
</template>

<style scoped>
#App {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 10px;
  padding: 20px;
}

.pokemon-card {
  text-align: center;
  border: 1px solid #ddd;
  border-radius: 10px;
  padding: 10px;
  background-color: #f9f9f9;
}

.pokemon-card img {
  width: 100px;
  height: 100px;
}

.pokemon-card p {
  font-size: 1rem;
  text-transform: capitalize;
  margin-top: 10px;
}

.pokemon-card input {
  margin-top: 10px;
  padding: 5px;
  width: 80%;
}

.pokemon-card button {
  margin-top: 10px;
  padding: 5px 10px;
  cursor: pointer;
}

.loading-message {
  text-align: center;
  font-size: 1.5rem;
  margin-top: 20px;
}
</style>


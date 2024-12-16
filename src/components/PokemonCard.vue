<template>
    <div class="pokemon-card">
      <img :src="pokemon.image" :alt="pokemon.name" :style="{ filter: pokemon.guessed ? 'none' : 'blur(5px) grayscale(100%)' }" />
      <div v-if="!pokemon.guessed">
        <input v-model="pokemon.input" placeholder="Ingresa el nombre..." />
        <button @click="verificarNombre">Adivinar</button>
      </div>
      <p v-if="pokemon.guessed">{{ pokemon.name }}</p>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      pokemon: Object, 
      onCorrectGuess: Function,
    },
    methods: {
      verificarNombre() {
        if (this.pokemon.name.toLowerCase() === this.pokemon.input.toLowerCase()) {
          this.pokemon.guessed = true; 
          this.onCorrectGuess(); 
        } else {
          alert("¡Nombre incorrecto! Intenta de nuevo.");
        }
      },
    },
  };
  </script>
  
  <style scoped>
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
  
  .pokemon-card p {
    font-size: 1rem;
    text-transform: capitalize;
    margin-top: 10px;
  }
  </style>
  
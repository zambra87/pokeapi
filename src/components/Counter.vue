<template>
    <div class="counter">
      <h1>Contador: {{ count }}</h1>
      <div class="buttons">
        <button @click="incrementar" class="btn btn-success">Incrementar</button>
        <button @click="decrementar" class="btn btn-danger">Decrementar</button>
        <button @click="reiniciar" class="btn btn-secondary">Reiniciar</button>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        count: 0,
      };
    },
    methods: {
      incrementar() {
        this.count++;
      },
      decrementar() {
        if (this.count > 0) this.count--;
      },
      reiniciar() {
        this.count = 0;
      },
    },
  };
  </script>
  
  <style scoped>
  .counter {
    text-align: center;
    margin-top: 50px;
  }
  
  .buttons button {
    margin: 10px;
  }
  </style>
  
<template>
    <div :class="cardClass">
      <h2>{{ nombre }}</h2>
      <p><strong>Fecha:</strong> {{ fecha }}</p>
      <p><strong>Hora:</strong> {{ hora }}</p>
      <p><strong>Gravedad:</strong> {{ gravedad }}</p>
      <p><strong>Motivo:</strong> {{ motivo }}</p>
  
      <button @click="emitirEliminar" class="boton-eliminar">Eliminar</button>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      nombre: String,
      fecha: String,
      hora: String,
      gravedad: String,
      motivo: String,
    },
    computed: {
      cardClass() {
        return {
          card: true,
          'gravedad-alta': this.gravedad === 'Alta',
          'gravedad-media': this.gravedad === 'Media',
          'gravedad-baja': this.gravedad === 'Baja',
        };
      },
    },
    methods: {
      emitirEliminar() {
        this.$emit('eliminar');
      }
    }
  };
  </script>
  
  <style scoped>
  .card {
    border: 1px solid #ccc;
    border-radius: 8px;
    padding: 10px;
    margin: 10px 0;
    background-color: #f9f9f9;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  }
  
  .gravedad-alta {
    background-color: #e74c3c; /* Rojo para alta gravedad */
    color: white;
  }
  
  .gravedad-media {
    background-color: #f1c40f; /* Amarillo para media gravedad */
  }
  
  .gravedad-baja {
    background-color: #2ecc71; /* Verde para baja gravedad */
  }
  
  .card h2 {
    margin-bottom: 10px;
    font-size: 18px;
  }
  
  .card p {
    margin: 5px 0;
  }
  
  .boton-eliminar {
    background-color: #e74c3c;
    color: white;
    border: none;
    padding: 8px 12px;
    border-radius: 4px;
    cursor: pointer;
  }
  
  .boton-eliminar:hover {
    background-color: #c0392b;
  }
  </style>
  
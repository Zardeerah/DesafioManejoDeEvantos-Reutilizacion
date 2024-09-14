<template>
  <div id="app">
    <div class="container">
      <div class="formulario-horizontal">
        <div class="form-group">
          <label :style="{ color: !nuevoUsuario.nombre ? '#E72929' : 'black', fontWeight: 'bold' }" for="nombre">
            Paciente
          </label>
          <input v-model="nuevoUsuario.nombre" type="text" id="nombre" class="input-largo" />
        </div>
        <div class="form-group">
          <label :style="{ color: !nuevoUsuario.fecha ? '#E72929' : 'black', fontWeight: 'bold' }" for="fecha">
            Fecha
          </label>
          <input v-model="nuevoUsuario.fecha" type="date" id="fecha" class="input-pequeno" />
        </div>
        <div class="form-group">
          <label :style="{ color: !nuevoUsuario.hora ? '#E72929' : 'black', fontWeight: 'bold' }" for="hora">
            Hora
          </label>
          <input v-model="nuevoUsuario.hora" type="time" id="hora" class="input-pequeno" />
        </div>
        <div class="form-group">
          <label :style="{ color: !gravedadOpciones ? '#E72929' : 'black', fontWeight: 'bold' }" for="gravedad">
            Gravedad
          </label>
          <select v-model="nuevoUsuario.gravedadSeleccionada" id="gravedad" class="input-pequeno">
            <option v-for="gravedad in gravedadOpciones" :key="gravedad">{{ gravedad }}</option>
          </select>
        </div>
        <div class="form-group">
          <label :style="{ color: !nuevoUsuario.motivo ? '#E72929' : 'black', fontWeight: 'bold' }" for="motivo">
            Motivo
          </label>
          <input v-model="nuevoUsuario.motivo" type="text" id="motivo" class="input-largo" />
        </div>
      </div>
      <div class="form-group form-boton">
        <button type="button" @click="agregarUsuario" class="boton-pequeno" :disabled="!formularioCompleto">
          Agregar
        </button>
      </div>


      <div v-if="usuarios.length === 0" class="mensaje">
        Aún no hay consultas registradas.
      </div>

      <div v-else class="usuarios">
        <div v-for="(usuario, index) in usuarios" :key="index"
          :class="['usuario', obtenerColorGravedad(usuario.gravedad)]">
          <UsuarioView :nombre="usuario.nombre" :fecha="usuario.fecha" :hora="usuario.hora" :gravedad="usuario.gravedad"
            :motivo="usuario.motivo" @eliminar="eliminarUsuario(index)" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import UsuarioView from './components/UsuarioView.vue';

export default {
  data() {
    return {
      nuevoUsuario: {
        nombre: "",
        fecha: "",
        hora: "",
        gravedadSeleccionada: 'Media', // Valor por defecto
        motivo: "",
      },
      gravedadOpciones: ['Baja', 'Media', 'Alta'],
      usuarios: [],
    };
  },
  components: {
    UsuarioView
  },
  computed: {
    formularioCompleto() {
      return this.nuevoUsuario.nombre &&
        this.nuevoUsuario.fecha &&
        this.nuevoUsuario.hora &&
        this.nuevoUsuario.motivo;
    }
  },
  methods: {
    agregarUsuario() {
      if (this.formularioCompleto) {
        const nuevoUsuario = {
          nombre: this.nuevoUsuario.nombre,
          fecha: this.nuevoUsuario.fecha,
          hora: this.nuevoUsuario.hora,
          gravedad: this.nuevoUsuario.gravedadSeleccionada,
          motivo: this.nuevoUsuario.motivo,
        };
        this.usuarios.push(nuevoUsuario);
        this.limpiarCampos();
      }
    },
    limpiarCampos() {
      this.nuevoUsuario = {
        nombre: "",
        fecha: "",
        hora: "",
        gravedadSeleccionada: 'Media',
        motivo: "",
      };
    },
    eliminarUsuario(index) {
      this.usuarios.splice(index, 1);
    },
    obtenerColorGravedad(gravedad) {
      switch (gravedad) {
        case 'Baja':
          return 'bg-verde';
        case 'Media':
          return 'bg-amarillo';
        case 'Alta':
          return 'bg-rojo';
        default:
          return '';
      }
    }
  }
}
</script>

<style>
.container {
  display: flex;
  flex-direction: column;
  max-width: 1000px;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 8px;
  background-color: #f9f9f9;
}

.formulario-horizontal {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 20px;
  margin-bottom: 20px;
}

.form-group {
  display: flex;
  flex-direction: column;
}

.form-group label {
  margin-bottom: 5px;
  font-weight: bold;
  color: black;
}

.label-rojo {
  color: red;
}

.input-pequeno {
  width: 120px;
}

.input-largo {
  width: 200px;
}

.form-group input,
.form-group select {
  padding: 8px;
  border-radius: 4px;
  border: 1px solid #ccc;
}

/* Botón Agregar */
.form-boton {
  display: flex;
  justify-content: center;
}

.boton-pequeno {
  padding: 5px 10px;
  font-size: 14px;
  width: 80px;
  border-radius: 4px;
  background-color: #eee;
  border: 1px solid #ccc;
  cursor: pointer;
}

.boton-pequeno:disabled {
  background-color: #ddd;
  cursor: not-allowed;
}

.boton-pequeno:hover:not(:disabled) {
  background-color: #ddd;
}

/* Mensajes y tarjetas */
.mensaje {
  text-align: center;
  color: #777;
  margin-top: 20px;
}

.usuarios {
  margin-top: 20px;
}

.bg-verde {
  background-color: #d4edda;
  border: 1px solid #c3e6cb;
}

.bg-amarillo {
  background-color: #fff3cd;
  border: 1px solid #ffeeba;
}

.bg-rojo {
  background-color: #f8d7da;
  border: 1px solid #f5c6cb;
}

.usuario {
  padding: 15px;
  margin-bottom: 10px;
  border-radius: 5px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}
</style>

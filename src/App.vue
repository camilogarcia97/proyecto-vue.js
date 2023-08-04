<template>
  <div class="container">
    <div class="form-container">
      <h2>Formulario de Registro</h2>
      <form @submit.prevent="submitForm">
        <div class="form-group">
          <label for="nombre">Nombre:</label>
          <input type="text" id="nombre" v-model="nombre" required> 
        </div>
        <div class="form-group">
          <label for="apellido">Apellido:</label>
          <input type="text" id="apellido" v-model="apellido" required>
        </div>
        <div class="form-group">
          <label for="correo">Correo:</label>
          <input type="email" id="correo" v-model="correo" required>
        </div>
        <div class="form-group">
          <label for="telefono">Teléfono Celular:</label>
          <input type="tel" id="telefono" v-model="telefono" required>
        </div>
        <div class="form-group">
          <label for="cargo">Cargo:</label>
          <input type="text" id="cargo" v-model="cargo" required>
        </div>
        <button type="submit">Enviar</button>
      </form>
    </div>
  </div>
  <div class="table-container">
      <h2>Registros Guardados</h2> 
      <table>
        <thead>
          <tr>
            <th>Nombre</th>
            <th>Apellido</th>
            <th>Correo</th>
            <th>Teléfono Celular</th>
            <th>Cargo</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(registro, index) in registros" :key="index">
            <td>{{ registro.nombre }}</td>
            <td>{{ registro.apellido }}</td>
            <td>{{ registro.correo }}</td>
            <td>{{ registro.telefono }}</td>
            <td>{{ registro.cargo }}</td>
          </tr>
        </tbody>
      </table>
    </div>
</template>

<script>
import axios from 'axios';

export default { //que es un export default
  data() {
    return {
      nombre: "",
      apellido: "",
      correo: "",
      telefono: "",
      cargo: "",
      registros: [], // Array para almacenar los registros guardados
    };
  },
  
  beforeCreate() {
    console.log('beforeCreate lifecycle hook');
      // Obtener los registros de la base de datos
      axios.get('api/obtener-registros')
        .then(response => {
          this.registros = response.data;
        })
        .catch(error => {
          console.error(error);
        });
  },

  methods: {
    
    submitForm() {
      // Aquí puedes enviar los datos del formulario al servidor o realizar alguna acción
      axios.post('api/guardar-registro', {
      nombre: this.nombre,
      apellido: this.apellido,
      correo: this.correo,
      telefono: this.telefono,
      cargo: this.cargo,
      })
      .then(response => {
        console.log('Formulario enviado!');
        // Recargar la tabla de registros con los datos guardados
        this.registros.push(response.data.registro);
      })
      .catch(error => {
        console.error(error);
      });
    },
  },
};
</script>

<style>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.form-container {
  color: black;
  background-color: #e4e4e4;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.2);
  max-width: 400px;
}

.form-group {
  margin-bottom: 20px;
}

label {
  font-weight: bold;
  color: black;
}

input {
  width: 100%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

button {
  padding: 10px 20px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #0056b3;
}

.table-container {
  color: black;
  margin-top: 30px;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 10px;
}

th,
td {
  border: 1px solid #3e3e3e;
  padding: 8px;
}

th {
  background-color: #e7e7e7;
  font-weight: bold;
}
</style>

<template>
  <div class="registro-container">
    <h2>Registro de Usuarios</h2>

    <div class="form-section">
      <input type="text" v-model="nombre" placeholder="Nombre" />
      <input type="text" v-model="apellido" placeholder="Apellido" />
      <input type="text" v-model="telefono" placeholder="Teléfono" />
      
      <button @click="guardarRegistro">
        {{ editId !== null ? 'Actualizar' : 'Guardar' }}
      </button>
    </div>
    
    <hr>

    <div class="table-section">
      <table>
        <thead>
          <tr>
            <th>Id</th>
            <th>Nombre</th>
            <th>Apellido</th>
            <th>Teléfono</th>
            <th>Opciones</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(item, index) in registro" :key="item.id">
            <td>{{ item.id }}</td>
            <td>{{ item.nombre }}</td>
            <td>{{ item.apellido }}</td>
            <td>{{ item.telefono }}</td>
            <td>
              <button @click="editarItem(item)">✏️</button>
              <button @click="eliminarItem(index)">❌</button>
            </td>
          </tr>
        </tbody>
      </table>

      <p v-if="registro.length === 0" class="no-data">No hay registros para mostrar.</p>
    </div>

  </div>
</template>

<script setup>
import { ref } from 'vue';

// 1. ESTADO REACTIVO: Todas estas variables, al ser ref(),
// si cambian, automáticamente actualizan la parte visual (template).
const nombre = ref('');
const apellido = ref('');
const telefono = ref('');
const registro = ref([]); // Almacén de los objetos de usuario
const editId = ref(null); // ID del item que se está editando

// 2. FUNCIONES DE LÓGICA


function guardarRegistro() {
  if (!nombre.value || !apellido.value || !telefono.value) {
    alert("Todos los campos son requeridos.");
    return;
  }

  // Lógica de EDICIÓN
  if (editId.value !== null) {
    const index = registro.value.findIndex(item => item.id === editId.value);
    if (index !== -1) {
      // Actualizar los datos del objeto en el array
      registro.value[index].nombre = nombre.value;
      registro.value[index].apellido = apellido.value;
      registro.value[index].telefono = telefono.value;
    }
  } 
  // Lógica de CREACIÓN
  else {
    const data = {
      id: Date.now(),
      nombre: nombre.value,
      apellido: apellido.value,
      telefono: telefono.value
    };
    registro.value.push(data); // Vue actualiza la tabla
  }

  limpiarFormulario(); // Resetear inputs y modo edición
}

/**
 * Carga los datos de un item en el formulario para su edición.
 * @param {Object} item - El objeto de registro a editar.
 */
function editarItem(item) {
  // Carga los datos en los ref() para que aparezcan en los inputs (gracias a v-model)
  nombre.value = item.nombre;
  apellido.value = item.apellido;
  telefono.value = item.telefono;
  editId.value = item.id; // Activa el modo de edición
}

/**
 * Elimina un registro del array por su índice.
 * @param {number} index - El índice del item a eliminar.
 */
function eliminarItem(index) {
  registro.value.splice(index, 1);
  // Vue detecta el cambio en el array y elimina la fila de la UI
}

/**
 * Resetea los valores de los inputs y el ID de edición.
 */
function limpiarFormulario() {
  nombre.value = '';
  apellido.value = '';
  telefono.value = '';
  editId.value = null;
}
</script>

<style >

.registro-container {
  max-width: 900px;
  margin: 40px auto;
  padding: 30px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 15px;
  box-shadow: 0 10px 30px rgba(0,0,0,0.2);
  color: #fff;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.registro-container h2 {
  text-align: center;
  margin-bottom: 30px;
  font-size: 2.5em;
  text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
}

.form-section {
  display: flex;
  flex-wrap: wrap;
  gap: 15px;
  margin-bottom: 30px;
  justify-content: center;
}

.form-section input {
  flex: 1 1 200px;
  padding: 12px 15px;
  border: none;
  border-radius: 25px;
  background: rgba(255, 255, 255, 0.9);
  color: #333;
  font-size: 16px;
  transition: all 0.3s ease;
  box-shadow: 0 4px 6px rgba(0,0,0,0.1);
}

.form-section input:focus {
  outline: none;
  background: #fff;
  box-shadow: 0 6px 12px rgba(0,0,0,0.2);
  transform: translateY(-2px);
}

.form-section input::placeholder {
  color: #666;
}

.form-section button {
  padding: 12px 30px;
  border: none;
  border-radius: 25px;
  background: linear-gradient(45deg, #ff6b6b, #ee5a24);
  color: white;
  font-size: 16px;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 15px rgba(255, 107, 107, 0.4);
  text-transform: uppercase;
  letter-spacing: 1px;
}

.form-section button:hover {
  transform: translateY(-3px);
  box-shadow: 0 6px 20px rgba(255, 107, 107, 0.6);
}

hr {
  border: none;
  height: 2px;
  background: linear-gradient(90deg, transparent, rgba(255,255,255,0.5), transparent);
  margin: 30px 0;
}

.table-section {
  overflow-x: auto;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
  background: rgba(255, 255, 255, 0.95);
  border-radius: 10px;
  overflow: hidden;
  box-shadow: 0 5px 15px rgba(0,0,0,0.1);
}

th, td {
  padding: 15px;
  text-align: left;
  border-bottom: 1px solid #e0e0e0;
}

td{
  color:#000;
}

th {
  background: linear-gradient(45deg, #4facfe 0%, #00f2fe 100%);
  color: white;
  font-weight: bold;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  position: sticky;
  top: 0;
}

tbody tr:hover {
  background: rgba(79, 172, 254, 0.1);
  transform: scale(1.01);
  transition: all 0.2s ease;
}

td button {
  margin-right: 5px;
  padding: 8px 12px;
  border: none;
  border-radius: 20px;
  cursor: pointer;
  font-size: 14px;
  transition: all 0.3s ease;
}

td button:first-child {
  background: #4ecdc4;
  color: white;
}

td button:first-child:hover {
  background: #45b7aa;
  transform: scale(1.1);
}

td button:last-child {
  background: #ff6b6b;
  color: white;
}

td button:last-child:hover {
  background: #ee5a24;
  transform: scale(1.1);
}

.no-data {
  text-align: center;
  color: rgba(255, 255, 255, 0.8);
  margin-top: 30px;
  font-style: italic;
  font-size: 18px;
}

/* Responsive design */
@media (max-width: 768px) {
  .registro-container {
    margin: 20px;
    padding: 20px;
  }

  .registro-container h2 {
    font-size: 2em;
  }

  .form-section {
    flex-direction: column;
  }

  .form-section input, .form-section button {
    width: 100%;
  }

  th, td {
    padding: 10px;
    font-size: 14px;
  }
}
</style>

<template>
  <body>
    <header class="container">
        <h1>vue</h1>
        <img alt="Vue logo" class="logo" src="../src/assets/logo.png" width="125" height="125" />
    </header>
    <main>
      <div class="container">
        <div class="row">
          <div class="col-md-12">
            <h1>Personas  🧚</h1>
          </div>
        </div>
        <div class="row">
          <div class="col-md-12">
            <!-- agregar componente -->

            <!-- capturar datos form -->
            <formulario-persona @add-persona="agregarPersona"/>
            <!-- luego pasar datos al componente -->
            <tabla-personas 
            :personas="personas" 
            @delete-persona="eliminarPersona" 
            @actualizar-persona="actualizarPersona"
            />
            <footer-firma />
          </div>
        </div>
      </div>
    </main>
  </body>


</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'

import TablaPersonas from './components/TablaPersonas.vue';
import FormularioPersona from './components/FormularioPersona.vue';
import FooterFirma from './components/FooterFirma.vue';

export default {
  name: 'app',
  components: {
    TablaPersonas,
    FormularioPersona,
    FooterFirma
  },
  // array con datos dinámicos
  data() {
    return {
      personas: [
        {
          id: 1,
          nombre: 'jon',
          apellido: 'nieve',
          email: 'jon@c.cl',
        },
        {
          id: 2,
          nombre: 'tyrion',
          apellido: 'lannister',
          email: 'tyrion@c.cl',
        },
        {
          id: 3,
          nombre: 'daenerys',
          apellido: 'targaryen',
          email: 'daenerys@c.cl',
        },
      ],
    }
  },
  methods: {
    agregarPersona(persona) {
      // para id unico
      let id = 0;
      if(this.personas.length > 0){
        id = this.personas[this.personas.length - 1].id + 1;
      }
      // hemos usado el operador spread de propagación ..., útil para combinar objetos y arrays, para crear un nuevo array que contenga los elemento antiguos del array personas junto con la nueva.
      this.personas = [...this.personas, {...persona, id}];
    },
    eliminarPersona(id){
      this.personas = this.personas.filter(persona => persona.id !== id);
    },
    actualizarPersona(id, personaActualizada){
      // map para recorrer array personas y actualiza coincidencia con id
      this.personas = this.personas.map(persona => persona.id === id ? personaActualizada : persona)
    }
  },
}


</script>


<style>
body{
  background-color: #03045e;
  color: white;
}

table {
  border: 2px solid #009435;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 15px;
}
</style>

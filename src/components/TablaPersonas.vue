<template lang="">
    <!-- pascal case para archvios -->
    <!-- kebab case para componentes -->
    <div id="tabla-personas" class="table-responsive">
        <div v-if="!personas.length" class="alert alert-info" role="alert">
            no existen personas
        </div>
        
       <table class="table table-hover table-bordered border-primary table-striped fst-italic">
        <caption class="text-white">List of users</caption>
        <thead>
            <tr class="fs-2">
                <th> nombre</th>
                <th> apellido</th>
                <th> email</th>
                <th> ▶️ 🎮</th>
            </tr>
        </thead>
        <tbody class="fs-4">
            <!-- bucle para mostrar los datos -->
            <tr v-for="persona in personas" :key="persona.id"> 
                <td v-if="editando === persona.id"> 
                    <input type="text" class="form-control" v-model="persona.nombre" />
                </td>
                <td v-else> 
                    {{ persona.nombre}}
                </td>
                <td v-if="editando === persona.id"> 
                    <input type="text" class="form-control" v-model="persona.apellido" />
                </td>
                <td v-else> 
                    {{ persona.apellido}}
                </td>
                <td v-if="editando === persona.id"> 
                    <input type="email" class="form-control" v-model="persona.email" />
                </td>
                <td v-else> 
                    {{ persona.email}}
                </td>
                 <!-- if else para editar -->
                <td v-if="editando === persona.id">
                    <div class="d-grid gap-2 d-md-block">
                        <!-- btn guardar -->
                        <button class="btn btn-outline-success btn-sm" @click="guardarPersona(persona)">💾 guardar</button>
                        <!-- btn cancelar -->
                        <button class="btn btn-outline-secondary btn-sm m-1" @click="cancelarEdicion(persona)">❌ cancelar</button>
                    </div>
               </td>
                <td v-else>
                    <div class="d-grid gap-2 d-md-block">
                        <!-- btn eliminar -->
                        <button class="btn btn-outline-danger btn-sm" @click="$emit('delete-persona', persona.id)">🗑️ eliminar</button>
                        <!-- btn editar -->
                        <button class="btn btn-outline-info btn-sm m-1" @click="editarPersona(persona)">🪄 editar</button>
                    </div>
                </td>
            </tr>
        </tbody>
       </table>
        
    </div>
</template>
<script>
export default {
    name: 'tabla-personas',
    // recibir datos del otro componente 
    // debe tener todas las propiedades a recibir
    // con nombre y tipo
    data() {
        return {
            editando: null,
        }
    },
    props: {
        personas: Array,
    },
    methods: {
        editarPersona(persona) {
            // se guardan datos originales
            this.personaEditada = Object.assign({}, persona);
            this.editando = persona.id;
        },
        guardarPersona(persona) {
            if(!persona.nombre.length || !persona.apellido.length || !persona.email.length){
                return;
            }
            // método actualizar
            this.$emit('actualizar-persona', persona.id, persona);
            this.editando = null;
        },
        cancelarEdicion(persona) {
            // se recuperan datos originales
            Object.assign(persona, this.personaEditada);
            this.editando = null;
        }
    },
}
</script>
<style lang="">

</style>
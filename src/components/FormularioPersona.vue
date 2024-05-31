<!-- formulario -->
<!-- el atributo v-model, que enlazará el valor de los campos con sus respectivas variables de estado -->
<template lang="">
        <div id="formulario-persona">
            <form @submit.prevent="enviarFormulario">
                <div class="container">
                    <div class="row fs-5">
                        <div class="col-md-4">
                            <div class="form-group">
                                <label> nombre</label>
                                <!-- ante error vuelve a nombre con ref -->
                                <input
                                ref="nombre"
                                v-model="persona.nombre"
                                type="text"
                                class="form-control"
                                :class="{ 'is-invalid': procesando && nombreInvalido }"
                                @focus="resetEstado"
                                @keypress="resetEstado"
                                />
                                <!-- hemos agregado un evento @keypress al campo nombre para que el estado se resetee cuando se pulse una tecla, puesto que el foco ya estará en dicho campo. -->
                            </div>
                        </div>
                        <div class="col-md-4">
                            <div class="form-group">
                                <label> apellido</label>
                                <input
                                v-model="persona.apellido" 
                                type="text" 
                                class="form-control" 
                                :class="{ 'is-invalid': procesando && apellidoInvalido}"
                                @focus="resetEstado"
                                />
                            </div>
                        </div>
                        <div class="col-md-4">
                            <div class="form-group">
                                <label> email</label>
                                <input 
                                v-model="persona.email" 
                                type="email" 
                                class="form-control" 
                                :class="{ 'is-invalid': procesando && emailInvalido}"
                                @focus="resetEstado"
                                />
                            </div>
                        </div>
                    </div>
                    <div class="row">
                        <div class="d-grid gap-2 col-6 mx-auto m-3">
                            <div class="form-group">
                                <button class="btn btn-success">agregar persona </button>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="container">
                    <div class="row">
                        <div class="col-md-12">
                            <div v-if="error && procesando" class="alert alert-danger" role="alert"> 
                                debe rellenar todos los campos
                            </div>
                            <div v-if="correcto" class="alert alert-success" role="alert"> 
                                se agregó correctamente!
                            </div>
                        </div>
                    </div>
                </div>
            </form>    
        </div>
</template>
<script>
export default {
    name: 'formulario-persona',
    data() {
        return {
            // validar si se envía formulario
            procesando: false,
            correcto: false,
            error: false,
            persona: {
                nombre: '',
                apellido: '',
                email: ''
            },
        }
    },
    // se envía método al componente
    methods: {
        enviarFormulario() {
            // console.log('funca')
            this.procesando = true;
            this.resetEstado();

            // verificar error
            if (this.nombreInvalido || this.apellidoInvalido || this.emailInvalido) {
                this.error = true;
                console.log('error');
                return;
            }

            // método $emit envía el nombre del evento que definamos y los datos que deseemos al componente en el que se ha renderizado el componente actual.
            this.$emit('add-persona', this.persona);
            // ante error vuelve a nombre con ref
            this.$refs.nombre.focus();
            // el evento debe nombrarse con kebab-case

            // en caso de no existir error
            this.error = false;
            this.correcto = true;
            this.procesando = false;

            // limpiar variables
            this.persona = {
                nombre: '',
                apellido: '',
                email: ''
            }
        },
        resetEstado() {
            this.correcto = false;
            this.error = false;
        },
    },
    // computed properties, que son funciones que se ejecutan automáticamente cuando se modifica el estado de alguna propiedad
    // validaciones
    computed: {
        nombreInvalido() {
            return this.persona.nombre.length < 1;
        },
        apellidoInvalido() {
            return this.persona.apellido.length < 1;
        },
        emailInvalido() {
            return this.persona.email.length < 1;
        },
    }
}
</script>
<style scoped>
form {
    margin-bottom: 2rem;
}
</style>
<template>
    <div id="formulario-persona">
        <form @submit.prevent="enviarFormulario">
            <!-- @submit.prevent="" establim l'event listener onSubmit i a més prevé que el formulari s'envie -->
            <div class="container">
                <div class="row">
                    <div class="col-md-4">
                        <div class="form-group">
                            <label>Nombre</label>
                            <input ref="nombre" v-model="persona.nombre" type="text" class="form-control"
                                :class="{ 'is-invalid': procesando && nombreInvalido }" @focus="resetEstado"
                                @keypress="resetEstado" />
                            <!-- @focus també es un event listener onFocus que cada cop que entrem al input crida a la funció resetEstado -->
                        </div>
                    </div>
                    <div class="col-md-4">
                        <div class="form-group">
                            <label>Apellido</label>
                            <input v-model="persona.apellido" type="text" class="form-control"
                                :class="{ 'is-invalid': procesando && apellidoInvalido }" @focus="resetEstado" />
                        </div>
                    </div>
                    <div class="col-md-4">
                        <div class="form-group">
                            <label>Email</label>
                            <input v-model="persona.email" type="email" class="form-control"
                                :class="{ 'is-invalid': procesando && emailInvalido }" @focus="resetEstado" />
                        </div>
                    </div>
                </div>
                <div class="row">
                    <div class="col-md-4">
                        <div class="form-group">
                            <button class="btn btn-primary">Añadir persona</button>
                        </div>
                    </div>
                </div>
            </div>
            <div class="container">
                <div class="row">
                    <div class="col-md-12">
                        <div v-if="error && procesando" class="alert alert-danger" role="alert">
                            Debes rellenar todos los campos!
                        </div>
                        <div v-if="correcto" class="alert alert-success" role="alert">
                            La persona ha sido agregada correctamente!
                        </div>
                    </div>
                </div>
            </div>
        </form>
    </div>
</template>

<script>
export default {
    name: "formulario-persona",
    data() {
        return {
            // Tornarem 3 varibles, procesando per si el formulari s'he enviat, correcto si tot anat bé i error per si ho ha hagut algun error.
            procesando: false,
            correcto: false,
            error: false,
            persona: {
                nombre: "",
                apellido: "",
                email: "",
            },
        };
    },
    methods: {
        enviarFormulario() {
            this.procesando = true;
            this.resetEstado();

            // Comprobamos la presencia de errores
            if (this.nombreInvalido || this.apellidoInvalido || this.emailInvalido) {
                this.error = true;
                return;
            }

            this.correcto = true;
            this.procesando = false;
            this.error = false;

            this.$emit("add-persona", this.persona);
            this.$refs.nombre.focus();

            this.persona = {
                nombre: "",
                apellido: "",
                email: "",
            };
        },
        resetEstado() {
            this.correcto = false;
            this.error = false;
        },
    },
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
    },
};
</script>

<style scoped>
form {
    margin-bottom: 2rem;
}

form .form-group {
    margin-bottom: 1rem;
}
</style>
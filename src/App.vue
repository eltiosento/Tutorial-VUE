<template>
  <div id="app" class="container">
    <div class="row">
      <div class="col-md-12">
        <h1>Personas</h1>
      </div>
    </div>
    <div class="row">
      <div class="col-md-12">
        <formulario-persona @add-persona="agregarPersona" /> <!-- Truco '@'' orella escoltem que mos diu el fill -->
        <tabla-personas :personas="datosPersonas" @delete-persona="eliminarPersona"
          @actualizar-persona="actualizarPersona" />
        <!-- Ací estem pintant la vista del fill TablaPersones i al mateix temps li estem pasant les dades de les persones -->
      </div>
    </div>
  </div>
</template>

<script>
import TablaPersonas from './components/TablaPersonas.vue'
import FormularioPersona from './components/FormularioPersona.vue'

export default {
  name: 'app',
  components: {
    // Cridem els components que anem a utilitzar
    TablaPersonas,
    FormularioPersona,
  },
  // Creem un array amb les dades de les persones,
  // el creem aci al PARE per a que els fills puguen accedir a les dades
  // per aixo no ho fem al fill FormularioPersona.vue i la resta de fills
  // que necessiten aquestes dades pueden accedir
  data() {
    return {
      datosPersonas: [
        {
          id: 1,
          nombre: 'Jon',
          apellido: 'Nieve',
          email: 'jon@email.com',
        },
        {
          id: 2,
          nombre: 'Tyrion',
          apellido: 'Lannister',
          email: 'tyrion@email.com',
        },
        {
          id: 3,
          nombre: 'Daenerys',
          apellido: 'Targaryen',
          email: 'daenerys@email.com',
        },
      ],
    }
  },
  methods: {
    // Ací estem rebent les dades del fill FormularioPersona.vue
    // Utilitzem l'operador spread per a combinar les dades que ja tenim amb les noves
    // en altres paraules concatenem objectes, al nostre cas un array d'objectes amb un objecte.
    agregarPersona(persona) {
      this.datosPersonas = [...this.datosPersonas, { ...persona, id: this.datosPersonas.length + 1 }];
    },
    // Amb filter estem quedant-nos amb les dades que no coincideixen amb l'id que rebem
    // De manera que aquelles persones que no coincideixen amb l'id que rebem seran eliminades.
    eliminarPersona(id) {
      this.datosPersonas = this.datosPersonas.filter(persona => persona.id !== id);
    },
    actualizarPersona(id, personaActualizada) {
      this.datosPersonas = this.datosPersonas.map(persona => persona.id === id ? personaActualizada : persona);
    },
    /*
    // Aquesta seria una altra forma de fer-ho
    agregarPersona(persona) {
      this.datosPersonas.push({
        id: this.datosPersonas.length + 1,
        nombre: persona.nombre,
        apellido: persona.apellido,
        email: persona.email,
      })
    },
    */

  },
};
</script>

<style>
button {
  background: #009435;
  border: 1px solid #009435;
}
</style>

<template>
  <div>
    <v-input>
      <v-select rounded background-color="white" height="60" hide-details :items="razasList" label="Raza"
        item-text="nombre" item-value="nombre"  v-model="selectedRaza">
      </v-select>
    </v-input>
    <v-dialog v-model="showrazasModal">
      <v-card width="800">
        <v-toolbar color="gd-primary-to-right" elevation="0">
          <v-toolbar-title class="white--text font-weight-light">Razas</v-toolbar-title>
          <v-spacer></v-spacer>
          <v-btn icon @click="showrazasModal = false">
            <v-icon color="white">mdi-close</v-icon>
          </v-btn>
        </v-toolbar>
        <v-card-text class="pa-3">
          <v-text-field solo dense label="color" v-model="raza.nombre">
          </v-text-field>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn class="white--text" color="gd-primary-to-right font-weight-light rounded-lg"
            @click="addRaza()">
            AGREGAR
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
  export default {
    props: {
      value: null
    },
    data() {
      return {
        raza: {
          nombre: ""
        },
        selectedRaza: {},
        razasList: [],
        showrazasModal: false
      }
    },
    created() {
      this.getRazas()
    },
    mounted() {
      this.selectedRaza = this.value
    },
    methods: {
      addRaza() {
        this.$axios.post(`/razas`, this.raza)
          .then((data) => {
            this.raza.nombre = ""
            this.showrazasModal = false
            this.selectedRaza = data.data.id
            this.getrazas()
          })
      },
      getRazas() {
        this.$axios.get('/razas')
          .then((data) => {
            this.razasList = data.data
            this.razasList.unshift({value:'',nombre:'Seleccione una opcion'})
          })
      }
    },
    watch: {
      selectedRaza(val) {
        this.$emit('input', val)
      }
    }
  }

</script>

<style>

</style>

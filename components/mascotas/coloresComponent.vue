<template>
  <div>
    <v-input>
      <v-select background-color="white" rounded height="60" hide-details :items="coloresList" label="Color"
        item-text="nombre" item-value="nombre"  v-model="selectedColor">
      </v-select>
    </v-input>
    <v-dialog v-model="showcoloresModal">
      <v-card width="800">
        <v-toolbar color="gd-primary-to-right" elevation="0">
          <v-toolbar-title class="white--text font-weight-light">Colores</v-toolbar-title>
          <v-spacer></v-spacer>
          <v-btn icon @click="showcoloresModal = false">
            <v-icon color="white">mdi-close</v-icon>
          </v-btn>
        </v-toolbar>
        <v-card-text class="pa-3">
          <v-text-field solo dense label="color" v-model="color.nombre">
          </v-text-field>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn class="white--text" color="gd-primary-to-right font-weight-light rounded-lg"
            @click="addColor()">
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
        color: {
          nombre: ""
        },
        selectedColor: {},
        coloresList: [],
        showcoloresModal: false
      }
    },
    created() {
      this.getColores()
    },
    mounted() {
      this.selectedColor = this.value
    },
    methods: {
      addColor() {
        this.$axios.post(`/colores`, this.color)
          .then((data) => {
            this.color.nombre = ""
            this.showcoloresModal = false
            this.selectedColor = data.data.id
            this.getcolores()
          })
      },
      getColores() {
        this.$axios.get('/colores')
          .then((data) => {
            this.coloresList = data.data
          })
      }
    },
    watch: {
      selectedColor(val) {
        this.$emit('input', val)
      }
    }
  }

</script>

<style>

</style>

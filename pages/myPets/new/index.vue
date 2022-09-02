<template>
  <v-container>
    <v-row>
      <v-col class="col-12">

        <v-card color="primary lighten-1" class="rounded-lg">
          <v-toolbar color="teal darken-3" elevation="0">
            <v-toolbar-title class="white--text font-weight-bold">
              Agregar mascota
            </v-toolbar-title>
          </v-toolbar>
          <v-card-text>
            <v-form ref="form">
              <v-row>
                <v-col class="col-12 col-md-12">
                  <v-text-field label="Nombre" hide-details  rounded outlined background-color="white"  v-model="mascota.nombre">
                  </v-text-field>
                </v-col>
                <v-col class="col-12 col-md-12">
                  <mascotasRazasComponent v-model="mascota.raza"></mascotasRazasComponent>
                </v-col>
                <v-col class="col-12 col-md-12">
                  <mascotasColoresComponent v-model="mascota.color"></mascotasColoresComponent>
                </v-col>
                <v-col class="col-12 col-md-12">
                  <v-text-field type="date"  label="Fecha de nacimiento" rounded outlined background-color="white"
                    v-model="mascota.fecha_nac">
                  </v-text-field>
                </v-col>
                <v-col class="col-12 col-md-12">
                  <v-select label="SEXO" :items="[{
                      text:'Macho',
                      value: 'M'
                    },{
                      text:'Hembra',
                      value: 'F'
                    },{
                      text:'INDEFINIDO',
                      value: 'C'
                    }]" rounded outlined background-color="white" v-model="mascota.sexo">
                  </v-select>
                </v-col>
                <v-col class="col-12">
                  <v-select label="ESPECIE" :items="['Perro','Gato','Otro']" rounded outlined background-color="white" v-model="mascota.especie">
                  </v-select>
                </v-col>
                <v-col class="col-12 col-md-12">
                  <v-textarea label="Observaciones" rounded outlined background-color="white" v-model="mascota.observaciones">
                  </v-textarea>
                </v-col>


                <v-col class="col-12">
                  <v-divider></v-divider>
                </v-col>
              </v-row>
            </v-form>
          </v-card-text>

          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="teal darken-3" width="50%" x-large class="mb-5 white--text font-weight-bold rounded-xl" @click="checkHandler()">
              Guardar&nbsp;<v-icon>mdi-content-save</v-icon>
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
  import moment from 'moment'
  export default {
    data() {
      return {
        mascota: {
          especie:{}
        },
        rules: {
          required: [value => !!value || 'Este campo es requerido'],
        },
        cuotasList: [],
      }
    },
    created() {},
    mounted() {
      if (this.value) {
        this.mascota = this.value
        if (this.mascota.fecha_nac == '1000-01-01') {
          this.$delete(this.mascota, 'fecha_nac')
        }
      }

    },
    methods: {
      checkHandler() {
        if (!this.$refs.form.validate()) return
        // DATE TO ISO FORMAT MOMENT 
        if (this.mascota.deceso) {
          this.mascota.deceso = moment(this.mascota.deceso).toISOString()
        } else {
          this.$delete(this.mascota, 'deceso')
        }

        if (this.mascota.fecha_nac) {
          this.mascota.fecha_nac = moment(this.mascota.fecha_nac).toISOString()
        } else {
          this.$delete(this.mascota, 'fecha_nac')
        }
        this.mascota.id_socio = this.$auth.user.id
        this.$axios.post('/mascotas', this.mascota).then(res => {
          this.$router.push('/myPets')

        })
      },

    },
    watch: {},
  }

</script>

<style lang="scss">
</style>

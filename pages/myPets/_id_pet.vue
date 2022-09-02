<template>
  <v-container>
    <v-row>
      <v-col class="col-12">
        <v-card color="primary lighten-1" class="rounded-lg">
          <v-card-text>
            <v-list color="transparent">
                <v-card outlined color="primary" v-for="item in consultaItems" :key="item.id">
                    <v-list-item >
                    <v-list-item-content>
                        <v-list-item-title class=" white--text font-weight-bold d-flex justify-space-between">
                            <span>{{formatDate(item.fecha)}}</span><span>{{formatHour(item.hora)}}</span>
                        </v-list-item-title>
                        <v-divider style="background:#cecece" class="my-3"></v-divider>
                        <v-list-item-title class="white--text font-weight-bold py-3">EOG</v-list-item-title>
                        <v-list-item-subtitle class="white--text py-3">{{item.EOG}}</v-list-item-subtitle>
                        <v-divider style="background:#cecece" class="my-3"></v-divider>
                        <v-list-item-title class="white--text font-weight-bold py-3">Anamnesis</v-list-item-title>
                        <v-list-item-subtitle class="white--text py-3">{{item.anamnesis}}</v-list-item-subtitle>
                        <v-divider style="background:#cecece" class="my-3"></v-divider>
                        <v-list-item-title class="white--text font-weight-bold py-3">Tratamiento</v-list-item-title>
                        <v-list-item-subtitle class="white--text py-3">{{item.tratamiento}}</v-list-item-subtitle>
                    </v-list-item-content>
                </v-list-item>

                </v-card>
            </v-list>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
  import moment from 'moment'
  export default {
    layout: 'simpleWithBackButton',
    data() {
      return {
        pet: {},
        consultaItems: [],
        headers: [{
          text: "Fecha",
          value: "fecha"
        }, {
          text: "Hora",
          value: "hora"
        }, {
          text: "EOG",
          value: "EOG"
        }, {
          text: "Anamnesis",
          value: "anamnesis"
        }, {
          text: "Examenes",
          value: "examenes"
        }, {
          text: "Tratamiento",
          value: "tratamiento"
        }],

      }
    },
    created() {
      this.setTitle()
      this.getAtencions()
    },
    methods: {
      setTitle() {
        this.pet = this.$auth.user.mascotas.find((pet) => pet.id == this.$route.params.id_pet)
        this.$store.dispatch('titlePages/setTitlePage', {
          text: `Consultas de ${this.pet.nombre}`,
          color: 'secondary'
        })
      },

      formatDate(date) {
        if (!date) return 'Fecha no asignada'
        return moment(date).format('DD/MM/YYYY')
      },
      formatHour(hour) {
        if (!hour) return 'Hora no asignada'
        let finalHour = hour.split(':')
        return `${finalHour[0]}:${finalHour[1]}`
      },
      getAtencions() {
        this.$axios.get(`/atencion?mascota=${this.$route.params.id_pet}`).then(res => {
          this.consultaItems = res.data
        })
      }
    }
  }

</script>

<style>

</style>

<template>
  <v-container>
    <v-row>
      <v-col class="col-12">
        <v-card color="primary lighten-1">
          <v-toolbar color="teal darken-3" elevation="0">
            <v-toolbar-title class="font-weight-bold white--text ">Agenda</v-toolbar-title>
            <v-spacer></v-spacer>
            <v-btn outlined color="white" @click="openCreateReservation= true">
              Hacer reserva
            </v-btn>

          </v-toolbar>
          <v-card-title class="white--text font-weight-bold text-subtitle-1">Seleccione un dia...</v-card-title>
          <v-card-title>
            <v-select solo v-model="type" dense hide-details class="font-weight-light" label="Tipo de calendario"
              :items="[{value:'month',text:'Mensual'},{value:'day',text:'Diario'}]"></v-select>
          </v-card-title>

          <v-card-text>
            <v-sheet height="400">
              <v-calendar locale="es" ref="calendar" :events="agendaList" v-model="focus" color="primary"
                :event-color="getEventColor" :type="type" :value="now" @click:more="viewDay"
                @click:date="viewDay"></v-calendar>
            </v-sheet>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
    <v-dialog v-model="openCreateReservation">
      <v-card>
        <v-toolbar class="font-weight-bold white--text mb-3" color="teal darken-3" elevation="0">
            <v-toolbar-title>Mi consulta</v-toolbar-title>
        </v-toolbar>
        <v-divider></v-divider>
        <v-card-text>
          <v-form ref="form">
            <v-select :items="['Higiene','Consulta','Medicacion','Otro']" :rules="rules.required"
              v-model="agenda.consulta.tipo_consulta" label="Tipo de consulta" outlined></v-select>
            <v-text-field type="date" label="Fecha deseada" v-model="agenda.fecha" :rules="rules.required" outlined>
            </v-text-field>
            <v-text-field type="time" label="Hora deseada (Opcional)" v-model="agenda.hora" :rules="rules.required"
              outlined></v-text-field>
            <v-textarea outlined label="Detalles (Indique los detalles de su consulta)" :rules="rules.required"
              v-model="agenda.detalles" :counter="250" maxlength="250"></v-textarea>
          </v-form>
        </v-card-text>
        <v-card-actions>
          <v-btn color="teal darken-3" class="font-weight-bold white--text" block rounded @click="createAgenda()">Enviar</v-btn>
        </v-card-actions>
      </v-card>

    </v-dialog>
  </v-container>
</template>

<script>
  export default {
    data() {
      return {
        agenda: {
            consulta:{}
        },
        openCreateReservation: false,
        agendaList: [],
        colors: ['blue', 'indigo', 'deep-purple', 'cyan', 'green', 'orange', 'grey darken-1'],
        now: null,
        focus: '',
        type: 'month',
        rules: {
          required: [value => !!value || 'Este campo es requerido.'],
          min: value => value.length >= 3 || 'Este campo debe tener al menos 3 caracteres.',
          max: value => value.length <= 250 || 'Este campo debe tener como maximo 250 caracteres.'
        }

      }
    },
    created() {
      this.$store.dispatch('titlePages/setTitlePage', {
        text: 'Agenda',
        color: 'secondary'
      })
      this.getAgenda()
    },
    methods: {
      viewDay({
        date
      }) {
        this.focus = date
        this.type = 'day'
      },
      prev() {
        this.$refs.calendar.prev()
        this.changedMonths--
        let date = moment(this.now).add(this.changedMonths, 'months').endOf('month').format('YYYY-MM-DD')
        this.$emit('prev', date)
      },
      next() {
        this.$refs.calendar.next()
        this.changedMonths++
        let date = moment(this.now).add(this.changedMonths, 'months').endOf('month').format('YYYY-MM-DD')
        console.log(date)
        this.$emit('next', date)
      },

      getAgenda() {
        this.$axios.get('/agendas/')
          .then((data) => {
            this.agendaList = data.data.map(agenda => {
            let date = (agenda.hora) ? agenda.fecha + " " + agenda.hora.split('.')[0] : agenda.fecha
            return {
              name: agenda.titulo ?? "Fecha no disponible",
              start: date,
              end: date,
              data: agenda,
              allDay: agenda.type == 'indisponible'
            }
          })
          })
      },
      createAgenda(){
        if(this.$refs.form.validate()){
            this.agenda.hora = this.agenda.hora +':00.00'
            this.agenda.consulta.socio = this.$auth.user.id
          this.$axios.post('/agendas', this.agenda)
            .then(response => {
              this.getAgenda()
              this.openCreateReservation = false
            })
            .catch(error => {
              console.log(error)
            })
        }
      },
      deleteAgenda(id) {
        this.$axios.delete('/agenda/' + id).then(res => {
          this.getAgenda()
        })
      },
      getEventColor(event) {
        return event.color
      },
    },
  }

</script>

<style>

</style>

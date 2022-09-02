<template>
  <v-container fill-height>
    <v-row>
      <v-col class="col-12">
        <v-form ref="form">
          <v-text-field name="cumpleaños" placeholder="¿Cuando es el cumpleaños de tu mascota?" type="date"
            class="rounded-lg" id="cumpleaños" v-model="cumpleaños" :rules="cumpleañosRules" required filled>
          </v-text-field>
        </v-form>
      </v-col>
      <v-col class="col-12">
        <v-btn class="font-weight-bold white--text" color="gradient-primary" @click="siguiente" block rounded>Siguiente
        </v-btn>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
  export default {
    layout: 'simpleWithBackButton',
    created() {
      this.$store.dispatch('titlePages/setTitlePage', {
        text: 'Cumpleaños',
        color: 'secondary',
      })
    },
    data() {
      return {
        cumpleaños: null,
        cumpleañosRules: [
          v => !!v || 'Cumpleaños es requerido',
        ]
      }
    },
    methods: {
      siguiente() {
        if (!this.$refs.form.validate()) return;
        this.$store.dispatch('myPets/setCumpleaños', this.cumpleaños)
        let data = new FormData()
        data.append('files.profile_picture', this.pet.profile_picture)
        this.$delete (this.pet,'profile_picture')
        data.append(`data`, JSON.stringify({
          ...this.pet,
          user: this.$auth.user.id
        }))
        this.$axios.post('/mascotas/', data, {
            headers: {
              'Content-Type': 'multipart/form-data'
            }
          })
          .then((data) => {
            this.$router.push(`/myPets/new/gps/${data.data.id}`)
          })

      },
    },
    computed: {
      pet() {
        return this.$store.getters['myPets/pet'];
      },
    }
  }

</script>

<style>
  #cumpleaños::placeholder {
    font-size: 75% !important;
    text-align: center !important;
  }

</style>

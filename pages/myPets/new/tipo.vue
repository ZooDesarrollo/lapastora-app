<template>
  <v-container fill-height>
    <v-row>
      <v-col class="col-6" @click="setType('Perro')">
        <pictureType defaultPetType="Perro" :petType="getPetType" pictureUrl="/dog.png">
        </pictureType>
      </v-col>
      <v-col class="col-6 text-center d-flex flex-column justify-center align-center" @click="setType('Gato')">
        <pictureType defaultPetType="Gato" :petType="getPetType" pictureUrl="/cat.png">
        </pictureType>
      </v-col>
      <v-col class="col-12 text-center d-flex flex-column justify-center align-center" @click="setType('Otro')">
        <pictureType defaultPetType="Otro" :petType="getPetType" pictureUrl="/paw.png">
        </pictureType>
      </v-col>
      <v-col class="col-12">
        <v-btn rounded block class="font-weight-bold white--text" color="gradient-primary" @click="next()">Siguiente
        </v-btn>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
  import pictureType from '~/components/Pets/pictureType.vue'
  export default {
    layout: 'simpleWithBackButton',
    components: {
      pictureType
    },
    data() {
      return {

        select: null,
        showOtroTipoDeMascota: false,
        valid: true,
        otroTipoDeMascota: [
          'Vaca', 'Caballo'
        ],
        otroTipoDeMascotaRules: [
          v => !!v || 'Tipo de mascota is required',
          v => (v && v.length > 0) || 'Name must be less than 10 characters',
        ],
      }
    },
    created() {
      this.$store.dispatch('titlePages/setTitlePage', {
        text: 'Tipo de mascota',
        color: 'secondary',
      })
    },
    methods: {
      setType(type) {
        console.log(type)
        this.$store.dispatch('myPets/setType', type)
      },
      next() {
        this.$router.push("/myPets/new/sexo/")
      },
    },
    computed: {
      getPetType() {
        return this.$store.getters["myPets/pet"].type

      }
    }

  }

</script>

<style scoped>
  .mt-100 {
    margin-top: 20%;
  }

  .full-height {
    height: 100%;
  }

  .active {
    opacity: 1;
  }

  .inactive {
    opacity: .5;
  }

</style>

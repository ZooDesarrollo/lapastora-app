<template>
  <v-container fluid>
    <v-row no-gutters>
      <v-col class="col-12 col-sm-12">
        <v-card color="primary lighten-1" v-if="$auth.user.mascotas.length === 0"
          class="d-flex flex-column justify-space-between">
          <v-card-title class="no-mascotas text-center white--text d-flex justify-center">No tienes mascotas registradas
          </v-card-title>
          <v-card-text class="d-flex flex-column text-center">
            <v-img src="/cat.png" aspect-ratio="2.5"></v-img>
          </v-card-text>
          <v-card-actions>
            <v-btn x-large block color="teal darken-3" class="white--text font-weight-bold text-h6 rounded-lg"
              to="/myPets/new">
              Agregar mascota&nbsp;<v-icon size="30">mdi-plus-circle</v-icon>
            </v-btn>
          </v-card-actions>
        </v-card>
        <v-card class="mb-2 elevation-5 rounded-lg" v-for="pet in $auth.user.mascotas" color="primary lighten-1"
          :key="pet.id">
          <v-card-title primary-title>
            <v-avatar class="mr-2">
              <img src="/pets/dog.png" v-if="pet.especie =='Perro'" alt="my_pet_figure">
              <img src="/pets/cat.png" v-else-if="pet.especie =='Gato'" alt="my_pet_figure">
              <img src="/pets/other.png" v-else alt="my_pet_figure">
            </v-avatar>
            <div class="d-flex flex-column">
              <h4 class="white--text">{{pet.nombre}}</h4>
            </div>
          </v-card-title>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="teal darken-3 white--text font-weight-bold" :to="`/myPets/${pet.id}`">Ver consultas
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
    <v-app-bar fixed bottom app grow class="white rounded-t-xl" height="48">
      <v-toolbar-items class="pa-1 d-flex justify-center full-width">
        <v-btn color="teal darken-3" class="white--text font-weight-bold rounded-lg elevation-6" to="myPets/new">
          AGREGAR MI MASCOTA&nbsp;<v-icon>mdi-plus-circle</v-icon>
        </v-btn>
      </v-toolbar-items>
    </v-app-bar>
  </v-container>
</template>

<script>
  export default {
    layout: 'simpleWithBackButton',
    components: {},
    data() {
      return {
        myPet: {},
        showMap: false,
        pets: [],
      }
    },
    created() {
      this.$store.dispatch('myPets/getPets')
      this.$store.dispatch('titlePages/setTitlePage', {
        text: 'Mis mascotas',
        color: 'secondary'
      })
    },
    async mounted() {
      await this.$auth.fetchUser()
    },
    methods: {},
  }

</script>

<style scoped>
  .no-mascotas {
    font-size: 30px;
    font-weight: bold;
  }

</style>

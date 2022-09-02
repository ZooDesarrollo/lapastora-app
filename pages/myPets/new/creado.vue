<template>
  <v-container class="fill-height d-flex justify-center flex-column align-center">
    <v-card class="rounded-xl">
      <v-card-text class="d-flex justify-center flex-column align-center">
        <v-icon color="success darken-1" size="100">mdi-check-circle</v-icon>
        <p class="font-weight-black">Gps y mascota asociados con exito!</p>
      </v-card-text>
      <v-card-actions>
        <v-btn color="gradient-primary font-weight-black" block rounded @click="next">Finalizar</v-btn>
      </v-card-actions>
    </v-card>
  </v-container>
</template>

<script>
  export default {
    layout: 'withBackButton',
    created() {
      this.$store.dispatch('titlePages/setTitlePage', {
        text: 'GPS AGREGADO!',
        color: 'secondary'
      })
    },
    methods: {
      next() {
        this.$store.dispatch('myPets/clean')
        this.$router.push('/myPets/map')
      }
    }
  }

</script>

<style scoped>
  .fill-height {
    height: calc(100vh - 64px) !important;
  }

</style>

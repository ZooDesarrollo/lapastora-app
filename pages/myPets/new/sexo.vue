<template>
  <v-container fill-height>
    <v-row>
      <v-col class="col-6 text-center">
        <div class="active text-center d-flex flex-column justify-center align-center" @click="sexPet = 'macho'">
          <v-avatar size="132" :color="(sexPet=='macho') ? 'red' : 'white' ">
            <v-avatar size="125" color="white">
              <v-avatar size="120" color="orange">
                 <v-icon size="50" color="white">mdi-gender-male</v-icon>
              </v-avatar>
            </v-avatar>
          </v-avatar>
          <span :class="(sexPet=='macho') ? 'red--text font-weight-black' : 'black--text font-weight-black' "
            style="font-size: 1.5em">Macho</span>
        </div>
      </v-col>
      <v-col class="col-6 text-center">
        <div class="active text-center d-flex flex-column justify-center align-center" @click="sexPet = 'hembra'">
          <v-avatar size="132" :color="(sexPet=='hembra') ? 'red' : 'white' ">
            <v-avatar size="125" color="white">
              <v-avatar size="120" color="blue">
                  <v-icon size="50" color="white">mdi-gender-female</v-icon>
              </v-avatar>
            </v-avatar>
          </v-avatar>
          <span :class="(sexPet=='hembra') ? 'red--text font-weight-black' : 'black--text font-weight-black' "
            style="font-size: 1.5em">Hembra</span>
        </div>
      </v-col>
      <v-col class="col-12 color-grey text-center">
        <h3>¿Castrado/a?</h3>
        <v-row class="pa-5">
          <v-col class="col-6 text-center">
            <v-btn color="blue" class="white--text" rounded block :disabled="(sexPet == '')" @click="siguiente(true)">Si
            </v-btn>
          </v-col>
          <v-col class="col-6 text-center">
            <v-btn color="warning" rounded block :disabled="(sexPet == '')" @click="siguiente(false)">No</v-btn>
          </v-col>
        </v-row>
        <div class="d-flex justify-space-around">
        </div>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
  export default {
    layout: 'simpleWithBackButton',
    created() {
      this.$store.dispatch('titlePages/setTitlePage', {
        text: 'Sexo',
        color: 'secondary',
      })
    },
    methods: {
      siguiente(val) {
        this.$store.dispatch("myPets/setCastrado", val)
        this.$router.push("/myPets/new/cumpleanos")
      }
    },
    computed: {
      sexPet: {
        set(val) {
          this.$store.dispatch("myPets/setSex", val)
        },
        get() {
          return this.$store.getters["myPets/pet"].sex
        }
      }
    }
  }

</script>

<style scoped>
  .color-grey {
    background: rgb(245, 245, 245);
  }

</style>

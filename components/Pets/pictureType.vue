<template>
  <div class="active text-center d-flex flex-column justify-center align-center">
    <v-avatar size="132" :color="(petType==defaultPetType) ? 'red' : 'white' ">
      <v-avatar size="125" color="white">
        <v-avatar size="120" color="orange">
          <v-img :src="pictureUrl" contain rounded></v-img>
        </v-avatar>
      </v-avatar>
    </v-avatar>
    <span :class="(petType==defaultPetType) ? 'red--text font-weight-black' : 'black--text font-weight-black' "
      style="font-size: 1.5em">{{defaultPetType}}</span>
  </div>
</template>

<script>
  export default {
      props:{
          pictureUrl:String,
          petType:String,
          defaultPetType:String
      }
  }

</script>

<style>

</style>

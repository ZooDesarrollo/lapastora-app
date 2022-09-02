<template>
  <v-card-actions>
    <v-btn text small :class="`font-weight-black ${textColor}--text`" @click.stop.prevent="likeOrDislike">
      <v-icon size="25" :color="textColor" left v-if="!publication.likeUser">mdi-thumb-up-outline</v-icon>
      <v-icon size="25" color="success" left v-else>mdi-thumb-up</v-icon> &nbsp;
      {{publication.likes}}
    </v-btn>
    <v-spacer></v-spacer>
    <v-btn text x-small :class="`font-weight-black ${textColor}--text`" @click="goToPublication()">
      <v-icon size="25" dark left>ion-ios-text</v-icon>&nbsp;&nbsp;
      {{publication.comentarios}}
    </v-btn>
  </v-card-actions>

</template>

<script>
  export default {
      props:{
          textColor:{
            default:'white',
            type:String
          },
          publication:Object,
          goToPublication:Function,
          likeOrDislike:Function
      }
  }

</script>

<style>

</style>

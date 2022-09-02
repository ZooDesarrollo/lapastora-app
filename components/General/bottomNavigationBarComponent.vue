<template>
<div>
  <v-bottom-navigation fixed app grow  class="white rounded-t-xl" height="48">
    <v-btn color="orange darken-3" to="/" icon min-width="50">
      <v-icon>mdi-home</v-icon>
    </v-btn>
    <v-btn value="orange darken-3" color="primary" to="/myPets" icon min-width="40">
      <v-icon>mdi-paw</v-icon>
    </v-btn>
    <v-btn  fab color="transparent" class="p-absolute" to="/myPets/new" min-width="50">
      <v-avatar color="teal darken-3" size="50" class="elevation-1">
        <v-icon color="white" size="20">mdi-plus</v-icon>
      </v-avatar>
    </v-btn> 
    <v-btn color="orange darken-3" to="/calendar" icon min-width="50">
      <v-icon>mdi-calendar</v-icon>
    </v-btn>
    <v-btn color="orange darken-3" @click="disconnect()" icon min-width="50">
      <v-icon>mdi-logout</v-icon>
    </v-btn>

  </v-bottom-navigation>
      <PostsCreateComponent v-model="openCreatePost"></PostsCreateComponent>
  </div>
</template>

<script>
  export default {
    name: 'BottomNavigationBar',
    data() {
      return {
        bottomNav: 'profile',
        petImg: '',
        openCreatePost: false,
      }
    },
    mounted() {
    },
    methods: {
      goToPets() {
        if (this.pets == 1) {
          return this.$router.push(`/myPets/gps/${pets[0].id}`)
        } else {
          return this.$router.push(`/myPets/`)
        }
      },
      disconnect() {
        this.$auth.logout()
      }
    },
    computed: {
      pets() {
        return this.$store.getters['myPets/pets']
      }
    }
  }

</script>

<style lang="scss">
  .p-absolute {
    position: absolute;
    top: -20px;
    border-style: dotted;
  }

</style>

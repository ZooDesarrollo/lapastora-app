<template>
  <v-container fluid class="pa-0">
    <v-row no-gutters>
      <v-col class="col-12 mb-2">
        <v-card color="primary" class="mb-1 rounded-0" elevation="0">
          <v-card-title class="text-h4 white--text">
            Comunidad la Pastora
          </v-card-title>
          <v-card-subtitle class="white--text text-subtitle-2">
            Respetar a los animales es una obligacion, amarlos es un privilegio. "Los veterinarios son los heroes de nuestras mascotas".
          </v-card-subtitle>
        </v-card>
      </v-col>
      <v-col class="col-12">
        <v-divider></v-divider>
      </v-col>
      <v-col class="col-12">
        <PostsListComponent v-model="publications">
        </PostsListComponent>
      </v-col>
    </v-row>
    <v-btn fab dark small absolute right color="teal darken-3" @click="openCreatePost= true">
        <v-icon>mdi-plus</v-icon>
      </v-btn>

    <PostsCreateComponent @newPublication="setNewPost($event)" v-model="openCreatePost"></PostsCreateComponent>
  </v-container>
</template>

<script>
  export default {
    data() {
      return {
        openCreatePost: false,
        publications: {
          random: {}
        },
        randomPublications: [],
        followersPublications: false,
        start_publicaciones: 0,
        limit_publicaciones: 12,
        search: {
          title_contains: null,
          _start: 0,
          _limit: 12,
          type: 'publication'
        }
      }
    },
    created() {
      this.$store.dispatch('titlePages/setTitlePage', {
        text: 'Home',
        color: 'secondary'
      })
      this.getPosts()
      this.$store.dispatch('myPets/getPets')
    },
    mounted() {
      this.$root.$on('newPublication', (e) => {
        this.publications.data.unshift(e)
      })
      this.deletePost()
    },
    methods: {
      deletePost() {
        this.$root.$on('deletePublication', (index) => {
          console.log(index)
          this.$delete(this.publications.data, index)
        });
      },

      async getPosts() {
        let params = {
          _start: this.start_publicaciones,
          _limit: this.limit_publicaciones
        }
        this.publications = await this.$axios.get('/publicaciones/', {
          params: this.search
        })
      }
    },
    computed: {
      pets() {
        return this.$store.getters['myPets/pets']
      }
    },
    watch: {
      followersPublications() {
        this.getPosts()
      },
      lostDogs() {
        this.getPosts()
      },
      search: {
        handler() {
          this.getPosts()
        },
        deep: true
      }
    },
  }

</script>

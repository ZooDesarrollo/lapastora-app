<template>
  <v-card color="primary" tile @click="goToPublication()">
    <template v-if="publication.main_picture!=null">
      <v-card-title class=" py-0 full-width">
        <v-list color="primary" class="full-width">
            <v-list-item class="grow">
              <v-list-item-avatar color="mr-2 white">
                <v-img class="elevation-6" v-if="publication.user.profile_picture!=null" @click="goToProfile" :src="publication.user.profile_picture.url"
                  aspect-ratio="2" round></v-img>
              </v-list-item-avatar>
              <v-list-item-content>
                <v-list-item-title class="font-weight-black text-left white--text">{{publication.user.username}}</v-list-item-title>
              </v-list-item-content>
              <v-list-item-action>
                <v-btn fab depressed x-small v-show="publication.myPublication"
                  @click.stop.prevent="deletePublication(publication.id)">
                  <v-icon color="red">mdi-delete</v-icon>
                </v-btn>
              </v-list-item-action>
            </v-list-item>
          </v-list>
          </v-card-title>
        <v-img aspect-ratio="2" lazy-src="/logo.png" gradient="to top right, #0000001c, #00000040"
          :src="mainPictureUrl()" max-height="200" class="d-flex align-end" cover>
        </v-img>
      <v-card-text class="font-weight-light pb-2 text-h6 font-weight-light white--text"
        v-if="publication.content!=null">
        {{publication.title}}
      </v-card-text>
      <v-card-subtitle class="white--text pt-1">
        {{publication.content.substr(0,80)}}
      </v-card-subtitle>
      <v-divider style="background:#cecece5e"></v-divider>
      <v-card-actions>
        <postComments :publication="publication" :goToPublication="goToPublication" :likeOrDislike="likeOrDislike">
        </postComments>
      </v-card-actions>
    </template>
    <v-card class="mb-3" flat v-else>
      <v-card class="rounded-lg">
        <v-card-text class="font-weight-bold black--text text-left" v-if="publication.content!=null">
          {{publication.content.substr(0,80)}}
        </v-card-text>
        <v-divider></v-divider>
        <postComments textColor="black" class="py-1" :publication="publication" :goToPublication="goToPublication"
          :likeOrDislike="likeOrDislike"></postComments>
      </v-card>
    </v-card>
  </v-card>
</template>

<script>
  import moment from 'moment';
  import postComments from '~/components/Posts/PostCard/postComments.vue';
  export default {
    components: {
      postComments
    },
    props: {
      index: Number,
      publicProp: {
        type: Object,
        default: {
          likes: [],
        }
      }
    },
    data() {
      return {
        publication: this.publicProp,
        openBottomFollow: false
      }
    },
    methods: {
      goToProfile() {
        return this.$router.push('account/user/profile/' + this.publication.user.id)
      },
      goToPublication() {
        if (this.publication.comments_active)
          return this.$router.push(`/post/${this.publication.id}`)
      },
      formatDate(date) {
        return moment(date).format('DD/MM/YYYY');
      },
      mainPictureUrl() {
        if (this.publication.main_picture != null)
          return this.$axios.defaults.baseURL + this.publication.main_picture.url
        else
          return '/logo.png'
      },
      likeOrDislike() {

        if (this.publication.likeUser) {
          this.$axios.delete(`/publicaciones/removeLike/${this.publication.id}`).then(response => {
            this.publication.likeUser = false
            this.publication.likes -= 1
          })
        } else {
          this.$axios.post(`/publicaciones/addLike/${this.publication.id}`).then(response => {
            this.likeId = response.data.id
            this.publication.likeUser = true
            this.publication.likes += 1
          })
        }
      },
      deletePublication(id, index) {
        this.$axios.delete(`/publicaciones/${id}/`)
          .then(() => {
            this.$root.$emit('deletePublication', this.index)
          })
      }
    }
  }

</script>

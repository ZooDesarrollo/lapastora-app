<template>
  <loggedIn-template>
    <template slot-scope="mainProps">
      <titleToolbars>
        <template>
          <v-btn fab x-small color="white mr-2" depressed @click="$router.go(-1)">
            <v-icon color="primary" size="25">
              mdi-chevron-left</v-icon>
          </v-btn>
          <v-toolbar-title class="white--text font-weight-black pl-0">
            {{mainProps.titles.text}}
          </v-toolbar-title>
        </template>
      </titleToolbars>
      <v-content class="primary">
        <nuxt style="margin-top:-15px;" />
      </v-content>
      <bottomNavigationBarComponent v-if="showBottomBar"></bottomNavigationBarComponent>
    </template>
  </loggedIn-template>
</template>

<script>
  import titleToolbars from "~/components/General/titleToolbars.vue"
  import loggedInTemplate from "~/layouts/templates/loggedIn.vue"
  import bottomNavigationBarComponent from '~/components/General/bottomNavigationBarComponent.vue'
  import { App } from '@capacitor/app';

  export default {
    middleware: "auth",
    components: {
      titleToolbars,
      loggedInTemplate,
      bottomNavigationBarComponent
    },
    created() {
      App.addListener('backButton', ({ canGoBack }) => {
        if(canGoBack){
          window.history.back();
        } else {
          App.exitApp();
        }
      });

    },
    computed: {
      showBottomBar() {
        return this.$store.getters['general/showBottomBar']
      }
    }
  }

</script>

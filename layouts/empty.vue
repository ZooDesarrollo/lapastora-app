<template>
    <v-app>
      <v-content class="background" style="background:#1c202c">
        <nuxt />
      </v-content>
    </v-app>
  </template>

<script>
    export default {
        // middleware(cntx) {
        //     if (cntx.$auth.loggedIn) {
        //         cntx.redirect("/")
        //     }
        // },
    }
</script>
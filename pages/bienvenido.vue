<template>
  <v-container  class="fill-height pa-12">
    <loginFormComponent></loginFormComponent>
  </v-container>
</template>

<script>
  export default {
    layout: 'empty',
    data() {
      return {
        section: 0,
        logo: 'logo.png',
      }
    },
    computed: {
      imgSection() {
        return this.imgs[this.section]
      },
      txtSection() {
        return this.txts[this.section]
      }
    },
    methods: {
      changeView() {
        if (this.section == 3) {
          this.$router.push('/home')
        } else {
          this.section++
        }
      }
    }
  }

</script>

<style scoped>
  .v-btn {
    padding: 200px;
  }

  .mb100 {
    margin-bottom: 100%;
  }

  .cover-photo {
    border-bottom-left-radius: 20px;
    border-bottom-right-radius: 20px;
  }

</style>

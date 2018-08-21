<template>
  <v-app id="inspire">

    <v-toolbar color="indigo" dark fixed app>
      <v-toolbar-title>Application</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-toolbar-items class="hidden-sm-and-down">
        <v-btn
          v-for="icon in icons"
          :key="icon"
          class="mx-3 white--text"
          icon
        >
          <v-icon size="24px">{{ icon }}</v-icon>
        </v-btn>
      </v-toolbar-items>
    </v-toolbar>

    <nuxt/>

  </v-app>
</template>

<script>
  export default {
    data: () => ({
      drawer: null,
      icons: [
        'fab fa-facebook',
        'fab fa-twitter',
        'fab fa-google-plus',
        'fab fa-linkedin',
        'fab fa-instagram'
      ]
    }),
  }
</script>
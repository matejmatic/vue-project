<template>
  <v-app>
    <nav>
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </nav>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
          transition="scale-transition"
          width="40"
        />
        <v-btn
            v-for="link in links"
            :key="link.route"
            router :to="link.route"
            text
          >
            {{ link.text }}
          </v-btn>
  
          <v-spacer></v-spacer>
         
      </div>

      <v-spacer></v-spacer>
        <v-icon>mdi-open-in-new</v-icon>
    </v-app-bar>

    <v-main>
      <router-view/>
    </v-main>
  </v-app>
</template>

<script>

export default {
  name: 'App',

  data: () => ({
    links: [
        { text: 'Home',  route: '/'},
        { text: 'About', route: '/about'},
    ]
  }),
};
</script>

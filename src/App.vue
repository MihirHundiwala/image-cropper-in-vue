<template>
  <v-app>
    <v-main>
      <HomePage/>
    </v-main>
  </v-app>
</template>

<script>
import HomePage from './components/HomePage.vue'

export default {
  name: 'App',

  components: {
    HomePage,
  },

  data: () => ({
    //
  }),
}
</script>

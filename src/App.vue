<template>
  <v-app>
    <v-main class="default-layout">
      <Navbar />
      <HelloWorld />
      <Footer />
    </v-main>
  </v-app>
</template>

<script>
import Navbar from '@/components/Navbar.vue'
import HelloWorld from './components/HelloWorld'
import Footer from '@/components/Footer.vue'

export default {
  name: 'App',

  components: {
    Navbar,
    HelloWorld,
    Footer,
  },

  data: () => ({
    //
  }),
}
</script>

<style lang="scss">
body,
html {
  overflow-x: hidden;
}

.default-layout {
  min-height: 100vh;
}
</style>

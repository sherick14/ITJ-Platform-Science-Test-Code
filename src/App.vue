<template>
  <v-app>
    <v-main class="default-layout">
      <Navbar />
      <Shipment />
      <Footer />
    </v-main>
  </v-app>
</template>

<script>
import Navbar from '@/components/Navbar.vue'
import Shipment from '@/components/Shipments'
import Footer from '@/components/Footer.vue'

export default {
  name: 'App',

  components: {
    Navbar,
    Shipment,
    Footer,
  },

  data: () => ({
    //
  }),
}
</script>

<style lang="scss">
.default-layout {
  min-height: 100vh;
}
</style>

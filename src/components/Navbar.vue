<template>
  <div>
    <v-toolbar class="align-center" color="black">
      <v-toolbar-title>
        <img width="134" height="64" :src="AcmeLogo" class="mt-3 ml-6" alt="Acme company logo" />
      </v-toolbar-title>
    </v-toolbar>
  </div>
</template>

<script>
import AcmeLogo from '@/assets/images/AcmeLogo.png'
export default {
  name: 'NavigationBar',
  data() {
    return {
      AcmeLogo: AcmeLogo,
    }
  },
}
</script>

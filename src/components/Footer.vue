<template>
  <v-footer absolute color="black test">
    <v-row class="white--text">
      <v-col cols="8">
        <label>© All rights reserved 2023 / ACME INC</label>
      </v-col>
      <v-col cols="4" class="text-right">
        <label>Powered by ITJ USA, Inc.</label>
      </v-col>
    </v-row>
  </v-footer>
</template>

<script>
export default {
  name: 'FooterComponent',
}
</script>

<style scoped>
.test {
  overflow-y: hidden;
}
</style>

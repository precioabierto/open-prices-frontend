<template>
  <v-footer>
    <v-row>
      <v-col cols="12" md="6" align="center">
        Precio Abierto
        <br />
        <v-btn v-for="source in sourceList" class="mr-1" size="x-small" rounded="xl" active :prepend-icon="source.icon" :href="source.url" target="_blank">
          {{ source.label }}
          <v-tooltip activator="parent" open-on-click location="top">{{ source.name }}</v-tooltip>
        </v-btn>
      </v-col>
      <v-col cols="12" md="6" align="center">
        <v-btn class="mx-2" variant="text" href="/acerca" target="_blank">{{ $t('Footer.About') }}</v-btn>
        <v-btn class="mx-2" variant="text" to="/stats">{{ $t('Footer.Stats') }}</v-btn>
        <v-btn class="mx-2" variant="text" href="https://github.com/precioabierto" target="_blank">Código</v-btn>
      </v-col>
    </v-row>
  </v-footer>
</template>

<style scoped>
.v-footer{
  max-height: 20%;
  background-color: #1E1E24;
  color: #fff
}
</style>

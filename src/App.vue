<template>
  <v-app>
    <v-app-bar
      app
      color="#7c21d9"
      dark
    >
      <v-row justify="center">
        <v-col cols="auto">
          <a href="https://www.apruebodignidad.cl" target="_blank" class="pa-2">
            <v-img
              src="/apruebo-dignidad-logo.png"
              alt="Logo Apruebo Dignidad"
              height="50"
              contain
              max-width="100"
            />  
          </a>
        </v-col>
      </v-row>
    </v-app-bar>
    <v-main>
      <Conteo/>
    </v-main>
    <v-footer>
      <a style="text-decoration: none" href="https://conteo.seguimos.cl" target="_blank" class="black--text">
        Clonada de 🌳 Conteo Seguimos - Desarrolladas por Vicente Gamboa
      </a>
    </v-footer>
  </v-app>
</template>

<script>
import Conteo from './components/Conteo';

export default {
  name: 'App',

  components: {
    Conteo,
  },

  data: () => ({
    //
  }),
};
</script>

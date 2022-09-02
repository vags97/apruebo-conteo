<template>
  <v-container>
    <v-dialog v-model="dialogLimpiar" max-width="600px">
      <v-card>
        <v-card-title>
          Limpiar
        </v-card-title>
        <v-card-subtitle>
          ¿Está seguro/a de limpiar los datos?
        </v-card-subtitle>
        <v-card-text>
          <v-row>
            <v-col cols="6">
              <v-btn block color="grey darken-2" dark  @click="dialogLimpiar = false">
                No
              </v-btn>
            </v-col>
            <v-col cols="6">
              <v-btn block color="warning" @click="reset">
                Sí
              </v-btn>
            </v-col>
          </v-row>
        </v-card-text>
      </v-card>
    </v-dialog>
    <v-dialog v-model="dialogCopiar" max-width="600px">
      <v-card>
        <v-card-title>
          <v-icon left>{{mdiClipboardText}}</v-icon>Copiar a Portapaples
        </v-card-title>
        <v-card-subtitle>
          Copiar resultados ingresados a portapapeles de su dispositivo
        </v-card-subtitle>
        <v-card-text style="white-space: pre-line" v-html="copyText" />
        <v-card-actions>
          <v-row>
            <v-col>
              <v-btn block @click="dialogCopiar=false" color="grey darken-2" dark>
                <v-icon left>
                  {{ mdiClose }}
                </v-icon>
                Cerrar
              </v-btn>
            </v-col>
            <v-col>
              <v-btn block @click="textToClipboard" color="success">
                <v-icon left>
                  {{ copied? mdiClipboardCheck: mdiClipboardText }}
                </v-icon>
                {{ copyBtnText }}
              </v-btn>
            </v-col>
          </v-row>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-row justify="center">
      <v-col cols="12" md="8" class="px-0">
        <v-card elevation="0">
          <v-card-title>
            Conteo
          </v-card-title>
          <v-card-subtitle>
            Apoyo al conteo de votos del plebiscito constitucional en Chile (2022). Recuerda que el principal objetivo como apoderade es defender y observar los votos, no necesariamente el conteo! La web solo guarda los votos en tu celular.
          </v-card-subtitle>
          <v-card-text>
            <v-simple-table v-show="lado==='diestro'">
              <template v-slot:default>
                <tbody>
                  <tr>
                    <td class="px-0">Apruebo</td>
                    <td>{{ votosApruebo }}</td>
                    <td class="text-center">
                      <v-btn @click="changeVoto(-1, 'apruebo')" color="error">
                        <v-icon>{{mdiMinus}}</v-icon>
                      </v-btn>
                    </td>
                    <td class="text-center">
                      <v-btn @click="changeVoto(1, 'apruebo')" color="success">
                        <v-icon>{{mdiPlus}}</v-icon>
                      </v-btn>
                    </td>
                  </tr>
                  <tr>
                    <td class="px-0">Rechazo</td>
                    <td>{{ votosRechazo }}</td>
                    <td class="text-center">
                      <v-btn @click="changeVoto(-1, 'rechazo')" color="error">
                        <v-icon>{{mdiMinus}}</v-icon>
                      </v-btn>
                    </td>
                    <td class="text-center">
                      <v-btn @click="changeVoto(1, 'rechazo')" color="success">
                        <v-icon>{{mdiPlus}}</v-icon>
                      </v-btn>
                    </td>
                  </tr>
                  <tr>
                    <td class="px-0">Nulo</td>
                    <td>{{ votosNulo }}</td>
                    <td class="text-center">
                      <v-btn @click="changeVoto(-1, 'nulo')" color="error">
                        <v-icon>{{mdiMinus}}</v-icon>
                      </v-btn>
                    </td>
                    <td class="text-center">
                      <v-btn @click="changeVoto(1, 'nulo')" color="success">
                        <v-icon>{{mdiPlus}}</v-icon>
                      </v-btn>
                    </td>
                  </tr>
                  <tr>
                    <td class="px-0">Blanco</td>
                    <td>{{ votosBlanco }}</td>
                    <td class="text-center">
                      <v-btn @click="changeVoto(-1, 'blanco')" color="error">
                        <v-icon>{{mdiMinus}}</v-icon>
                      </v-btn>
                    </td>
                    <td class="text-center">
                      <v-btn @click="changeVoto(1, 'blanco')" color="success">
                        <v-icon>{{mdiPlus}}</v-icon>
                      </v-btn>
                    </td>
                  </tr>
                </tbody>
              </template>
            </v-simple-table>
            <v-simple-table v-show="lado==='zurdo'">
              <template v-slot:default>
                <tbody>
                  <tr>
                    <td class="text-center">
                      <v-btn @click="changeVoto(-1, 'apruebo')" color="error">
                        <v-icon>{{mdiMinus}}</v-icon>
                      </v-btn>
                    </td>
                    <td class="text-center">
                      <v-btn @click="changeVoto(1, 'apruebo')" color="success">
                        <v-icon>{{mdiPlus}}</v-icon>
                      </v-btn>
                    </td>
                    <td class="px-0">Apruebo</td>
                    <td>{{ votosApruebo }}</td>
                  </tr>
                  <tr>
                    <td class="text-center">
                      <v-btn @click="changeVoto(-1, 'rechazo')" color="error">
                        <v-icon>{{mdiMinus}}</v-icon>
                      </v-btn>
                    </td>
                    <td class="text-center">
                      <v-btn @click="changeVoto(1, 'rechazo')" color="success">
                        <v-icon>{{mdiPlus}}</v-icon>
                      </v-btn>
                    </td>
                    <td class="px-0">Rechazo</td>
                    <td>{{ votosRechazo }}</td>
                  </tr>
                  <tr>
                    <td class="text-center">
                      <v-btn @click="changeVoto(-1, 'nulo')" color="error">
                        <v-icon>{{mdiMinus}}</v-icon>
                      </v-btn>
                    </td>
                    <td class="text-center">
                      <v-btn @click="changeVoto(1, 'nulo')" color="success">
                        <v-icon>{{mdiPlus}}</v-icon>
                      </v-btn>
                    </td>
                    <td class="px-0">Nulo</td>
                    <td>{{ votosNulo }}</td>
                  </tr>
                  <tr>
                    <td class="text-center">
                      <v-btn @click="changeVoto(-1, 'blanco')" color="error">
                        <v-icon>{{mdiMinus}}</v-icon>
                      </v-btn>
                    </td>
                    <td class="text-center">
                      <v-btn @click="changeVoto(1, 'blanco')" color="success">
                        <v-icon>{{mdiPlus}}</v-icon>
                      </v-btn>
                    </td>
                    <td class="px-0">Blanco</td>
                    <td>{{ votosBlanco }}</td>
                  </tr>
                </tbody>
              </template>
            </v-simple-table>
          </v-card-text>
          <p class="text-center"><b>Votos totales:</b> {{ votosTotal }}</p>
          <v-card-actions>
            <v-row justify="center">
              <v-col cols="12" lg="4" md="8" sm="8">
                <v-btn color="success" block @click="dialogCopiar=true">
                  <v-icon left>
                    {{ mdiClipboardText }}
                  </v-icon>
                  Copiar a Portapapeles
                </v-btn>
              </v-col>
              <v-col cols="12" lg="4" md="6" sm="6">
                <v-btn color="grey darken-2" dark block @click="lado === 'diestro'? lado = 'zurdo': lado = 'diestro'">
                  <v-icon left>
                    {{ lado === 'diestro'? mdiArrowLeft: mdiArrowRight }}
                  </v-icon>
                  Cambiar Lado
                </v-btn>
              </v-col>
              <v-col cols="12" lg="4" md="6" sm="6">
                <v-btn color="warning" block @click="dialogLimpiar=true">
                  <v-icon left>
                    {{ mdiDelete }}
                  </v-icon>
                  Limpiar
                </v-btn>
              </v-col>
            </v-row>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import { mdiPlus, mdiMinus, mdiDelete, mdiClose, mdiArrowLeft, mdiArrowRight, mdiClipboardText,mdiClipboardCheck } from '@mdi/js'

export default {
  name: 'Conteo',
  data(){
    return {
      mdiPlus,
      mdiMinus,
      mdiDelete,
      mdiClose,
      mdiArrowLeft,
      mdiArrowRight,
      mdiClipboardText,
      mdiClipboardCheck,
      votosApruebo: 0,
      votosRechazo: 0,
      votosNulo: 0,
      votosBlanco: 0,
      dialogLimpiar: false,
      dialogCopiar: false,
      copyBtnText: 'Copiar',
      copied: false,
      lado: 'diestro'
    }
  },
  mounted() {
    if (localStorage.votosApruebo) {
      this.votosApruebo = parseInt(localStorage.votosApruebo) || 0;
    }
    if (localStorage.votosRechazo) {
      this.votosRechazo = parseInt(localStorage.votosRechazo) || 0;
    }
    if (localStorage.votosNulo) {
      this.votosNulo = parseInt(localStorage.votosNulo) || 0;
    }
    if (localStorage.votosBlanco) {
      this.votosBlanco = parseInt(localStorage.votosBlanco) || 0;
    }
  },
  watch: {
    votosApruebo(avotosApruebo) {
      localStorage.votosApruebo = avotosApruebo;
    },
    votosRechazo(avotosRechazo) {
      localStorage.votosRechazo = avotosRechazo;
    },
    votosNulo(aVotosNulo) {
      localStorage.votosNulo = aVotosNulo;
    },
    votosBlanco(aVotosBlanco) {
      localStorage.votosBlanco = aVotosBlanco;
    },
    dialogCopiar(){
      this.copyBtnText = 'Copiar';
      this.copied = false;
    }
  },
  computed: {
   votosTotal() {
    return this.votosApruebo + this.votosRechazo + this.votosNulo + this.votosBlanco;
   },
   copyText(){
    return 'Votos:\n'+
      'Apruebo: ' + this.votosApruebo + '\n' +
      'Rechazo: ' + this.votosRechazo + '\n' +
      'Nulo: ' + this.votosNulo + '\n' +
      'Blanco: ' + this.votosBlanco
   }
  },
  methods: {
    textToClipboard(){
      navigator.clipboard.writeText(this.copyText).then(()=> {
        console.log('Async: Copying to clipboard was successful!');
        this.copyBtnText = 'Copia Correcta';
        this.copied = true;
      }, function(err) {
        alert('No se pudo copiar texto: ', err);
      });
    },
    reset() {
      this.votosApruebo = 0
      this.votosRechazo = 0
      this.votosNulo = 0
      this.votosBlanco = 0
      this.dialogLimpiar = false
    },
    changeVoto(numero, tipo) {
      let options;
      if(numero > 0){
        options = {
          duration: 100,
          interval: 0,
          count: 1
        }
      } else {
        options = {
          duration: 50,
          interval: 10,
          count: 2
        }
      }
      this.initVibrate(options)
      switch (tipo) {
        case 'apruebo':
          if (this.votosApruebo + numero < 0) return this.votosApruebo = 0
          return this.votosApruebo += numero
        case 'rechazo':
          if (this.votosRechazo + numero < 0) return this.votosRechazo = 0
          return this.votosRechazo += numero
        case 'nulo':
          if (this.votosNulo + numero < 0) return this.votosNulo = 0
          return this.votosNulo += numero
        case 'blanco':
          if (this.votosBlanco + numero < 0) return this.votosBlanco = 0
          return this.votosBlanco += numero
      }
    },
    initVibrate(options = { duration: 100, interval: 100, count: 1 }) {
      if (!window) {
        return;
      }

      if (!window.navigator) {
        return;
      }

      if (!window.navigator.vibrate) {
        return;
      }
      const pattern = [];
      for (let index = 0; index < options.count; index++) {
        pattern.push(options.duration);
        pattern.push(options.interval);
      }
      window.navigator.vibrate(pattern);
    },
  }
}
</script>

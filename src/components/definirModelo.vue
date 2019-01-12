<template>
  <v-container>
    <v-content>
    <v-dialog v-model="alerta" max-width="500px" persistent>
			<v-card>
				<v-card-title class="headline">No hay Registros</v-card-title>
				<v-card-text>
					Antes debes cargar los datos del formato al modelo.
				</v-card-text>
				<v-card-actions>
					<v-btn color="primary" flat to="/UsingIA/PrepararDatos" @click="alerta = false">Cargar Datos</v-btn>
				</v-card-actions>
			</v-card>
		</v-dialog>
    <v-layout row wrap>
      <v-flex column xs12 sm12 md12>
        <h1 class="pb-2" style="font-size: 35px">Define el Modelo Cognitivo</h1>
        <v-divider></v-divider>
      </v-flex>
    </v-layout>
    <v-layout row wrap class="pt-5">
      <v-flex column xs12 sm12 md12 class="py-3">
        <v-card light hover style="border-color: black">
          <v-card-title primary-title>
            <!-- {{$store.state.app.application.user}} -->
            <!-- {{$store.state.app.application.authenticated}} -->
            <div class="headline">Datos del Modelo</div>
          </v-card-title>
          <!-- nombre del modelo -->
          <v-card-text>
            <v-text-field label="Dale un nombre a tu Modelo" hint="Ejemplo: Modelo de peticiones acerca de rubros presupuestales"
            v-model="$store.state.app.model.contexto.nombre" placeholder color="accent"
            :rules="[v => !!v || 'Campo requerido']"></v-text-field>
            <!-- descripción -->
            <v-textarea rows="2" label="Cuéntanos para qué sirve tu Modelo" v-model="$store.state.app.model.contexto.descripcion"
            hint="Ejemplo: Mi modelo permite obtener respuestas acerca de rubros presupuestales,procesos de rubros, adición
            presupuestal, etc." color="accent" @keydown.tab="tabFocus"></v-textarea>
            <!-- palabras clave -->
            <v-tooltip v-model="view" top>
              <v-text-field slot="activator" label="Palabra Clave" @keyup.enter="addPalabraClave" v-model="palabraClave"
              placeholder color="accent" :persistent-hint="perH"></v-text-field>
              <span>Presiona ENTER para agregar las palabras una a una. Debes agregar como mínimo tres palabras clave.</span>
            </v-tooltip>
            <div>
              <v-chip v-for="(val, index) in this.$store.state.app.model.contexto.palabrasClave" :key="index"
              @input="deleteWord(index)" close color="grey" dark small style="font-weigth: bold">{{val}}</v-chip>
            </div>
          </v-card-text>
          <div class="text-xs-center">
            <v-btn v-if="$store.state.app.application.createModel" :disabled="enableSaveModel" color="black" class="accent--text" @click="sendModel">Crear Modelo</v-btn>
            <v-btn v-else :disabled="enableSaveModel" color="black" class="error--text" @click="UpdateModel">Actualizar Modelo</v-btn>
          </div>
        </v-card>
      </v-flex>
    </v-layout>
    <div class="text-xs-justify pt-4">
      <v-btn color="black" left absolute outline to="/UsingIA/PrepararDatos">Atrás</v-btn>
      <v-btn color="black" v-if="!$store.state.app.application.createModel" right absolute outline to="/UsingIA/palabraClave">Siguiente</v-btn>
    </div>
    </v-content>
  </v-container>

</template>

<script>
import EventBus from '@/components/EventBus'
import { url } from '../main'

export default {
  name: 'DefinirModelo',
  mounted () {
    if (this.$store.state.app.application.mode === 'DIY' && this.$store.state.app.model.registros.length === 0) {
      this.alerta = true
    }
  },
  data: () => ({
    palabraClave: '',
    loading: false,
    alerta: false,
    view: false,
    // url: 'https://carinag-225014.appspot.com',
    text: 'Presiona ENTER para agregar las palabras una a una/ Palabras agregadas: ',
    perH: false
  }),
  methods: {
    tabFocus () {
      this.view = true
    },
    deleteWord (index) {
      this.$store.commit('app/deleteWord', index)
    },
    addPalabraClave () {
      this.$store.commit('app/addPalabraClave', this.palabraClave)
      this.palabraClave = ''
      this.perH = true
    },
    sendModel () {
      EventBus.$emit('loading', {loading: true})
      this.$store.dispatch('app/addingCognitiveModel', url)
    },
    UpdateModel () {
      EventBus.$emit('loading', {loading: true})
      this.$store.dispatch('app/updatingCognitiveModel')
    }
  },
  computed: {
    enableSaveModel () {
      if (this.$store.state.app.model.contexto.nombre !== '' && this.$store.state.app.model.contexto.palabrasClave.length >= 3) {
        return false
      } else {
        return true
      }
    },
    addedWords () {
      let words = ''
      this.$store.state.app.model.contexto.palabrasClave.forEach(word => {
        words += `${word}, `
      })
      return this.text + words.slice(0, -2)
    }
  },
  components: { EventBus }
}
</script>

<style lang="css">
</style>

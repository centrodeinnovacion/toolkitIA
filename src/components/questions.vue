<template>
	<v-container>
	<v-content>
		<v-dialog v-model="editQuestionText" max-width="500px" persistent>
			<v-card>
				<v-card-title class="headline">Agregar Categoria</v-card-title>
				<v-card-text>
					<v-textarea rows="2" label="Palabra a agregar" v-model="question"
					placeholder color="accent" :persistent-hint="true"></v-textarea>
				</v-card-text>
				<v-card-actions>
					<v-btn color="primary" flat @click="editQuestionText = false, obj.data.pregunta = question">Aceptar</v-btn>
					<v-btn color="primary" flat @click="editQuestionText = false">Cancelar</v-btn>
				</v-card-actions>
			</v-card>
		</v-dialog>

    <h1 class="pb-2" style="font-size: 35px">Preguntas</h1>
		<v-divider></v-divider>
		<v-layout row wrap class="mt-5">
      <v-flex column xs12 sm6 md12>
					<div>
			      <v-alert :value="true" icon="info" color="primary" outline dismissible><strong>
			        Recuerda...</strong><br>Puedes descartar las preguntas haciendo click en el checkbox.
			      </v-alert>
			    </div>
					<v-layout row wrap>
						<v-flex column xs12 sm4 md4>
							<v-card class="elevation-7" height="300px" width="100%" v-if="this.$store.state.app.model.registros.length !== 0">
								<v-card-text class="list">
									<v-list dense>
											<v-list-tile v-for="(value, i) in selectQuestions" :key="i" :class="{activo: value.active }"
											@click="index = value.val, wordR = value.val"> {{value.val}}</v-list-tile>
									</v-list>
								</v-card-text>
							</v-card>
						</v-flex>
						<v-flex column xs12 sm8 md8>
							<v-card class="pa-3" height="100%" width="100%">
								<div v-if="index === null" style="font-size: 16px; color: grey; text-align: center" class="pa-5 ma-5">
									Por favor, seleccione una palabra relevante para mostrar la pregunta y respuesta asociada a esta.
								</div>
								<div v-else>
									<v-list-tile v-for="(val, index) in checkQuestions" :key="index">
										<div v-if="val.preguntasGeneradas !== undefined" style="inline: block">
											<!-- <v-list-tile-title> -->
												<v-btn icon v-if="val.preguntasGeneradas.seleccionado !== true" @click="findValue(index, val)"><v-icon>check_box_outline_blank</v-icon></v-btn>
												<v-btn icon v-else @click="findValue(index, val)"><v-icon color="accent">check_box</v-icon></v-btn>
												{{val.preguntasGeneradas.pregunta}}
											<!-- </v-list-tile-title> -->
										</div>
									</v-list-tile>
								</div>
							</v-card>
						</v-flex>
					</v-layout>
				<!-- </v-card> -->
			</v-flex>
		</v-layout>
		<div class="text-xs-center pa-5">
			<v-btn @click="sendWords()" color="black" class="accent--text">Enviar Relaciones y Preguntas</v-btn>
		</div>
    <!-- <div class="text-xs-center">
      <v-btn color="black" outline @click="next('/UsingIA/questionsList')">Siguiente</v-btn>
    </div> -->
		<div class="text-xs-justify pt-4">
      <v-btn color="black" left absolute outline to="/UsingIA/temasDeInteres">Atrás</v-btn>
    </div>
		</v-content>
  </v-container>
</template>

<script>
import EventBus from '@/components/EventBus'
import { url } from '../main'

export default {
  name: 'questions',
  data: () => ({
    checkbox: false,
    index: null,
    question: '',
    wordR: null,
    editQuestionText: false
  }),
  methods: {
    sendWords () {
      EventBus.$emit('loading', {loading: true})
      this.$store.dispatch('app/relationedWords', url)
    },
    findValue (index, q) {
      let a = this.$store.state.app.model.registros.indexOf(q)
      // console.log(a)
      this.$store.commit('app/addWordToRegister', { val: this.index, index: index, q: a })
    },
    next (to) {
      let el = this.$store.state.app.application.usingCM.steps['P'].data.find(el => el.to === to)
      this.$store.commit('app/changeElements', {to: to, el: el, i: 'P'})
      this.$store.commit('app/changeStep', 'P')
      EventBus.$emit('init', 'P')
    },
    editQuestion () {
      this.question = this.obj.data.pregunta
      this.editQuestionText = true
    }
  },
  computed: {
    selectQuestions () {
      let array = []
      // console.log(this.$store.state.app.model)
      this.$store.state.app.model.contexto.palabrasClave.map(el => {
        let e = {active: null, val: el}
        if (el === this.wordR) {
          e.active = true
        } else {
          e.active = false
        }
        array.push(e)
      })
      return array
    },
    checkQuestions () {
      let array = []
      this.$store.state.app.model.registros.map(register => {
        if (register.preguntasGeneradas !== undefined) {
          // console.log(register.preguntasGeneradas)
          if (register.preguntasGeneradas.palabrasRelevantes !== undefined) {
            if (register.preguntasGeneradas.palabrasRelevantes.indexOf(this.index) !== -1) {
              register.preguntasGeneradas.seleccionado = true
            } else {
              register.preguntasGeneradas.seleccionado = false
            }
          }
          array.push(register)
        }
      })
      return array
    }
  },
  components: { EventBus }
}
</script>

<style lang="css">
.activo {
	background-color: rgb(53, 204, 204, 0.2);
	border-radius: 15px
}
.activo:hover {
	background-color: rgb(53, 204, 204, 0.2);
	border-radius: 15px
}
.list{min-height:78.5%; max-height:78.5%; width:100%; list-style:none;}
.list{overflow:hidden; overflow-y:scroll;}
</style>

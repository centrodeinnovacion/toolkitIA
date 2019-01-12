<template>
	<v-container>
	<v-content>
		<!-- <dialogo :addWord="addWord"/> -->
    <h1 class="pb-2" style="font-size: 35px">Pregúntas y Respuestas</h1>
		<v-divider/>
				<v-layout row wrap class="mt-5">
		      <v-flex column xs12 sm12 md12>
						<v-card class="pa-3" height="100%" width="100%">
							<v-card-text>
								<v-layout row wrap>
								<!--<v-tooltip bottom>-->
									<v-btn v-if="!view" slot="activator" icon @click="view = !view"><v-icon>keyboard_arrow_right</v-icon></v-btn>
									<!--<span>Ver preguntas frecuentes</span>-->
								<!--</v-tooltip>-->
									<v-flex column xs12 sm5 md5 v-if="view">
										<v-card-title style="text-align: center">
											<h3 class="mb-0" style="font-size: 22px">Preguntas Frecuentes</h3>
											<v-btn v-if="view === false" icon @click="view = true"><v-icon>keyboard_arrow_right</v-icon></v-btn>
											<v-btn v-else icon @click="view = false"><v-icon>keyboard_arrow_left</v-icon></v-btn>
										</v-card-title>
										<v-list two-line subheader v-if="view === true">
											<v-list-tile v-for="(val, index) in questions" :key="index" @click="obj = val">
											  <v-list-tile-content style="cursor: pointer">
										    	<p>{{val.questions}}</p>
							          </v-list-tile-content>
											</v-list-tile>
										</v-list>
									</v-flex>
									<v-divider vertical/>
									<v-flex column xs12 sm5 md5>
										<v-card-title style="text-align: center">
											<h3 class="mb-0" style="font-size: 22px">Buscar Pregunta</h3>
										</v-card-title>
										<v-list two-line subheader>
											<v-list-tile>
												<v-textarea rows="2" label="Aquí la pregunta a buscar" v-model="question" required ></v-textarea>
											</v-list-tile>
											<v-list-tile v-for="(value, index) in filterQuestions" :key="index" v-if="question !== ''" @click="question = value.pregunta, obj2 = value">
												<v-list-tile-content>
													<p>{{value.questions}}</p>
												</v-list-tile-content>
											</v-list-tile>
										</v-list>
										<div v-if="obj2 !== null" class="mt-3">
											<h6 class="mb-0" style="font-size: 18px">Pregunta</h6>
											<p>{{obj2.questions}}</p>
											<h6 class="mb-0" style="font-size: 18px">Respuesta</h6>
											<p>{{obj2.respuesta}}</p>
										</div>
									</v-flex>
								</v-layout>
							</v-card-text>
						</v-card>
					</v-flex>
				</v-layout>
			</v-flex>
		</v-layout>
		<div class="text-xs-justify pt-4">
      <v-btn color="black" left absolute outline to="/UsingIA/questions">Atrás</v-btn>
    </div>
		</v-content>
  </v-container>
</template>

<script>
export default {
  name: 'questionsList',
  data: () => ({
    question: '',
    obj: {},
    obj2: null,
    view: false
  }),
  computed: {
    questions () {
      let array = []
      this.$store.state.app.model.registros.map(el => {
        if (el.preguntasGeneradas !== undefined) {
          let q = el.preguntasGeneradas.pregunta
          let o = {questions: q, respuesta: el.preguntasGeneradas.respuesta}
          if (array.indexOf(o) === -1) {
            array.push(o)
          }
        }
      })
      return array
    },
    filterQuestions () {
      return this.questions.filter(el => el.questions.includes(this.question))
    }
  }
}
</script>

<style lang="css">
</style>

<template>
	<v-footer height="auto" color="black">
    <v-card class="flex" flat tile>
			<v-layout class="black darken-3" justify-center row wrap >
				<v-btn color="white" target="_blank" flat round href="https://github.com/centrodeinnovacion/toolkitIA">
          <v-layout row wrap>
            <v-flex column>
              Descarga el Código fuente
            </v-flex>
            <v-flex column>
              <v-img class="ml-2" :src="githubIcon" width="22px" height="22px" style="background-color: white; border-radius: 50%;"/>
            </v-flex>
          </v-layout>
        </v-btn>
				<!--<v-btn color="white" flat round>Contáctenos</v-btn>
				<v-btn color="white" flat round>Términos y Condiciones</v-btn>-->
			</v-layout>
      <v-card-title class="black">
				<v-img max-width max-height contain :src="logoCompleto"></v-img>
      </v-card-title>
    </v-card>
  </v-footer>
</template>

<script>
import logoPNUD from '../assets/LOGOpnud.png'
import logoGobierno from '../assets/logoMINTIC.png'
import logoCentroInnovacion from '../assets/centroInnovacion.png'
import logoCompleto from '../assets/sinfondo.png'
import githubIcon from '../assets/github.png'

export default {
  name: 'footerToolkit',
  data () {
    return {
      fixed: false,
      logoPNUD,
      logoGobierno,
      logoCentroInnovacion,
      logoCompleto,
      githubIcon
    }
  }
}
</script>

<style lang="css">
</style>

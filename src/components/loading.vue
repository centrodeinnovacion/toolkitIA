<template>
	<div>
    <v-dialog v-model="dialog" width="500" persistent>
      <v-card color="info" dark>
        <v-card-text>
        Carina está procesando tus datos...
        <v-progress-linear indeterminate color="white" class="mb-0"></v-progress-linear>
        <br>
        <p v-if="loading.item !== undefined" style="text-align: center">Procesado {{loading.item}} de {{loading.long}} registros</p>
        </v-card-text>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  name: 'loading',
  props: [ 'loading' ],
  data () {
    return {
      dialog: true
    }
  }
}
</script>

<style lang="css">
</style>

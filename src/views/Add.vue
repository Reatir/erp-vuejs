<template>
  <div class="add">
      <Formulaire @newIntervention="handleNewIntervention"/>
  </div>
</template>

<script>

import Formulaire from '../components/Formulaire.vue'
import axios from 'axios'
export default {
  name: 'Add',
  components:{
      Formulaire

  },
  methods:{
      handleNewIntervention(inter){
        const strBody = {title: inter.titre, description: inter.description, operator: inter.technicien, completed: false}
        axios.post('http://localhost:8080/interventions/', strBody, {headers: {}}).then(response => {this.interventions = response.data })
      }
  }
}
</script>

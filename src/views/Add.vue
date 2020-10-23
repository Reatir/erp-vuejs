<template>
  <div class="add">
      <Formulaire @newIntervention="handleNewIntervention"/>
  </div>
</template>

<script>

import Formulaire from '../components/Formulaire.vue'
export default {
  name: 'Add',
  components:{
      Formulaire

  },
  methods:{
      handleNewIntervention(inter){
          console.log(inter);
      }
  }
}
</script>

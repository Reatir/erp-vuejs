<template>

      <form id="addintervention-form" method="post" @submit.prevent="handleSubmit" >
          
        <input
        type="text"
        v-model="titre"
        placeholder="Saisir titre"
      />
        <br> <br>
      <input
        type="text"
        v-model="description"
        placeholder="Saisir description"
      />
       <br> <br>
        <input
        type="text"
        v-model="technicien"
        placeholder="Saisir technicien"
      />
    <br> <br>
      <input type="submit" id="submit" value="envoyer" />
    </form>

</template>

<script>
export default {
    name: 'Formulaire',
    data(){
        return {
            titre: '',
            description: '',
            technicien: '',

        }
    },
    methods: {
        handleSubmit(){
            const intervention = {
                titre: this.titre,
                description: this.description,
                technicien: this.technicien,
            };
            this.$emit('newIntervention',intervention);
        }

    }

}


</script>
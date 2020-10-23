<template>
    <form id="addintervention-form" method="post" @submit.prevent="handleSubmit" >
        <label for="title">Titre </label>
        <input
        type="text"
        v-model="title"
      />
        <br> <br>
        <label for="description">Description </label>
      <textarea
        type="text"
        v-model="description"
        
      />
       <br> <br>
        <label for="technicien">Technicien </label>
        <input
        type="text"
        v-model="technicien"
       
      />
        <br> <br>

        <label for="completed">Completé </label>
        <input
        type="checkbox"
        v-model="completed"
       
      />
    <br> <br>
      <input type="submit" id="submit" value="envoyer" />
    </form>



</template>

<script>

import interventionsJson from '../../data/interventions.json'

export default {
    name: 'Formulaire',
    data(){
        return {
            title:interventionsJson[this.$route.params.id].title,
            description: interventionsJson[this.$route.params.id].description,
            technicien: interventionsJson[this.$route.params.id].operator,
            completed: !interventionsJson[this.$route.params.id].completed,
        }
    },
    methods: {
        handleSubmit(){
             //interventionsJson[this.$route.params.id].title="ok";
        }

    }

}


</script>
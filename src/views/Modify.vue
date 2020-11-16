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

import axios from 'axios'

export default {
    name: 'Formulaire',
    data(){
        return {
            id:0,
            title:'',
            description:'',
            technicien:'',
            completed: false,
        }
    },

    created(){
      const id = this.$route.params.id;
      this.id = id;
      axios.get('http://localhost:8080/interventions/' + id,{headers: {}}).then(response => { this.title = response.data.title, this.description = response.data.description,
      this.technicien = response.data.operator, this.completed = response.data.completed})

    },
    methods: {
        handleSubmit(){
          const strBody = {title: this.title, description: this.description, operator: this.technicien, completed: this.completed}
          axios.put('http://localhost:8080/interventions/' + this.id, strBody, {headers: {}}).then(response => {this.interventions = response.data })
        }

    }

}


</script>
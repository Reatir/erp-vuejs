<template>
  <tr @dblclick="HandleFastEdit">
    <td>
      {{ id }}
    </td>
    <td>
      <span v-if="!edit">{{ title }}</span>
      <input v-else type="text" v-model="modifyTitle"/>
    </td>
        <td>
       <span v-if="!edit">{{description }}</span>
      <textarea v-else type="text" v-model="modifyDescription"/>
    </td>
    <td>
      <span v-if="!edit">{{ operator }}</span>
      <input v-else type="text"  v-model="modifyOperator"/>
      
    </td>
    <td>
     
       <input v-if="!edit" onclick=" return false;" type="checkbox" v-model="completed"/>
       <input v-else type="checkbox" v-model="modifyCompleted"/>
    </td>
    <td>
      <router-link  style="width : 100px;height : 50px; background-color: #4CAF50" :to="{name: 'modify', params:{id: this.id}}" tag="button" >Modify</router-link>
    </td>
    <td>
      <button @click.prevent="handleDelete"  style="width : 100px;height : 50px; background-color: #DC143C">Delete</button>
    </td>
  </tr>
</template>

<script>
export default {
    name: "Intervention",

    props:[
        'id',
        'title',
        'description',
        'operator',
        'completed',
    ],
    data() {
      return {
        modifyTitle:this.title,
        modifyDescription:this.description,
        modifyOperator: this.operator,
        modifyCompleted: this.completed,
        edit: false
      }

    },
    methods: {
        handleDelete() {
           this.$emit("deleteIntervention",this.id);
        },
        HandleFastEdit() {
          this.edit =  ! this.edit;
          if( this.edit)
          {
            this.handleEdit();
          }
        },
        bindEscape()
        {
          document.addEventListener("keyup", e => {
            
            if(e.keyCode === 27 && this.edit == true)
            {
                this.edit =false;
                this.handleEdit();
            }
            });
        },
        handleEdit()
        {
           this.$emit("editIntervention",this.id,this.modifyTitle,this.modifyDescription,this.modifyOperator,this.modifyCompleted);
        }
    },
    mounted(){
      this.bindEscape()
    }

};
</script>
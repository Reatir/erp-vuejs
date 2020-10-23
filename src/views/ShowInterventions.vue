<template>
  <div class="showinterventions">
    <label>Recherche : </label>
    <input id="search" v-model="search" />
   <select v-model="stateSearch"> name="search">
    <option value=1>Global</option>
    <option value=2>Id</option>
    <option value=3>Title</option>
    <option value=4>Description</option>
    <option value=5>Technicien</option>
    <option value=6>Complété</option>  
  </select>
    <table>
      <thead>
        <tr>
          <th @click="SortId">Id {{ columnSortId }}</th>
          <th @click="SortTitle">Title {{ columnSortTitle }}</th>
          <th>Description</th>
          <th @click="SortTechnicien">technicien {{ columnSortTechnicien }}</th>
          <th @click="SortCompleted"> Complété {{ columnSortCompleted}}</th>
          <th colspan=2 >Actions</th>
        </tr>
       
      </thead>

      <tbody>
        <!-- <Intervention  v-for="(row,index) in interventions" :key="index" :id="row.id" :title="row.title" :description="row.description" :operator="row.operator" :completed="row.completed"/> -->
        <Intervention
          v-for="(row, index) in testinterventions"
          :key="index"
          :id="row.id"
          :title="row.title"
          :description="row.description"
          :operator="row.operator"
          :completed="row.completed"
          @deleteIntervention="deleteIntervention"
          @editIntervention="editIntervention"
        />
      </tbody>

      <tfoot>
        <tr>
          <label>Nombre de résultats : {{ length }}</label>
          <br /><br />
          <button @click="decreaseCurrentPage">-</button>
          <label>{{ currentPage }}</label>
          <button @click="increaseCurrentPage">+</button>
        </tr>
      </tfoot>
    </table>
  </div>
</template>

<script>
import interventionsJson from "../../data/interventions.json";
import Intervention from "../components/Intervention.vue";
import Fuse from "fuse.js";

export default {
  name: "ShowInterventions",
  data() {
    return {
      // fuseIntervention: new Fuse(interventionsJson),
      // interventions: fuseIntervention.search(""),
      interventions: interventionsJson,
      search: "",
      oldsearch: "",
      length: 0,
      step: 10,
      currentPage: 1,
      PageMax: 0,
      columnSortId: 0,
      columnSortTitle: 0,
      columnSortTechnicien: 0,
      columnSortCompleted: 0,
      stateSearch:"1",
    };
  },
  computed: {
    testinterventions() {
      if (this.oldsearch != this.search) {
        this.currentPage = 1;
        this.oldsearch = this.search;
      }
      const results = this.searchResults();
      const resultsSort = this.SortIntervention(results);
      this.length = resultsSort.length;
      this.PageMax = Math.ceil(this.length / this.step);

      const ListIntervention = [];

      const borneMax = Math.min(this.currentPage * this.step, this.length);

      for (
        let index = (this.currentPage - 1) * this.step;
        index < borneMax;
        index++
      ) {
        ListIntervention.push(resultsSort[index]);
      }
      console.log(this.length);
      return ListIntervention;
    },
  },
  methods: {
    SortIntervention(resultsSort) {
      switch (this.columnSortId) {
        case 0:
             resultsSort = resultsSort.sort((a, b) => a.id - b.id);
            break;
        case 1:
          resultsSort = resultsSort.sort((a, b) => a.id - b.id);
          break;
        case 2:
          resultsSort = resultsSort.sort((a, b) => b.id - a.id);
          break;
      }

      switch (this.columnSortTitle) {

        case 0:
          break;

        case 1:
          resultsSort = resultsSort.sort((a, b) => {
            if (a.title < b.title) { return -1;}
            if (a.title > b.title) { return 1;}
            return 0;
          });
          break;

        case 2:
          resultsSort = resultsSort.sort((a, b) => {
            if (a.title < b.title) { return 1;}
            if (a.title > b.title) { return -1;}
            return 0;
          });
          break;


      }

      switch (this.columnSortTechnicien) {

        case 0:
          break;

        case 1:
          resultsSort = resultsSort.sort((a, b) => {
            if (a.operator < b.operator) { return -1;}
            if (a.operator > b.operator) { return 1;}
            return 0;
          });
          break;

        case 2:
          resultsSort = resultsSort.sort((a, b) => {
            if (a.operator < b.operator) { return 1;}
            if (a.operator > b.operator) { return -1;}
            return 0;
          });
          break;


      }

    switch (this.columnSortCompleted) {
         case 0:
            break;
        case 1:
          resultsSort = resultsSort.sort((a, b) => a.completed - b.completed);
          break;
        case 2:
          resultsSort = resultsSort.sort((a, b) => b.completed- a.completed);
          break;
      }
    
        return resultsSort;
    
      
    },
    searchResults() {
       
      if (this.search.length == 0) {
        return this.interventions;
      }

     
      const fuseIntervention = new Fuse(this.interventions, {
        threshold: 0.1,
        includeScore: true,
        keys: this.getKey(),
      });
        
      const search = fuseIntervention.search( this.search);
      const results = search.map((result) => result.item);
      return results;
    },

    increaseCurrentPage() {
      if (this.currentPage < this.PageMax) {
        this.currentPage++;
      }
    },
    decreaseCurrentPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
      }
    },
    deleteIntervention(id) {
      this.interventions = this.interventions.filter((inter) => inter.id != id);
    },
    editIntervention(id,modifyTitle,modifyDescriptionodifyOperator,modifyCompleted)
    {
         this.interventions = this.interventions.filter((inter) => {
             if(inter.id === id)
             {
                 inter.title = modifyTitle;
                 inter.description= modifyDescription;
                 inter.operator = modifyOperator
                 inter.completed = modifyCompleted
             }
             return inter;
         });
    },
    SortId() {
      if (this.columnSortId < 2) {
        this.columnSortId++;
      } else {
        this.columnSortId = 0;
      }
    },
    SortTitle() {
      if (this.columnSortTitle < 2) {
        this.columnSortTitle++;
      } else {
        this.columnSortTitle = 0;
      }
    },
    SortTechnicien() {
      if (this.columnSortTechnicien < 2) {
        this.columnSortTechnicien++;
      } else {
        this.columnSortTechnicien = 0;
      }
    },
    SortCompleted() {
      if (this.columnSortCompleted < 2) {
        this.columnSortCompleted++;
      } else {
        this.columnSortCompleted = 0;
      }
    },
    getKey() {
        console.log(this.stateSearch);
        switch (this.stateSearch) {
            case "1":
                return ["id", "title", "operator", "description", "completed"];
                break;
            case "2":
                return ["id"];
                break;
            case "3":
                return ["title"];
                break;
            case "4":
                return ["description"];
                break;
            case "5":
                return ["operator"];
                 break;
            case "6":
                return ["completed"];
                break;
        }
    }
  },

  components: {
    Intervention,
  },
};
</script>
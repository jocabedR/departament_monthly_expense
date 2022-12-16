<template>
  <ul>
    <div id="geneal-information">
      {{ model.name }}: $300.00
      <button @click="deleteManager(model.id)" title="Delete Manager" v-if="model.id != 0"> - </button>
      <Total :total="calculateTotal"/>
    </div>

   <li id="list">
      <button @click="toggleDevelopers"> {{ developersOpen ? '«' : '»' }} </button>
      <span>{{model.developers.length}} Developer(s)</span>
      <button @click="addDeveloper" title="Add Developer"> + </button>
      <ul v-if="developersOpen">
        <li v-for="developer in model.developers" :key="developer.id">
          <span>{{developer.name}} : $1,000.00</span>
          <button @click="deleteDeveloper(developer.id)" title="Delete Developer"> - </button>
        </li>
      </ul>
    </li>

    <li>
      <button @click="toggleQATesters"> {{ qa_testersOpen ? '«' : '»' }} </button>
      <span>{{model.qa_testers.length}} QA Tester(s)</span>
      <button @click="addQATester" title="Add QA Tester"> + </button>
      <ul v-if="qa_testersOpen">
        <li v-for="qa_tester in model.qa_testers" :key="qa_tester.id">
          <span>{{qa_tester.name}} : $500.00</span>
          <button @click="deleteQATester(qa_tester.id)" title="Delete QA Tester"> - </button>
        </li>
      </ul>
    </li>

    <li>
      <button @click="toggleChildren"> {{ childrenOpen ? '«' : '»' }} </button>
      <span>{{model.children.length}} Manager(s)</span>
      <button @click="addChild" title="Add Manager"> + </button>
      <ul v-if="childrenOpen">
        <Child 
          v-for="child in model.children"
          :key="child.id"
          :model="child" 
          :managers="this.auxiliar"
          id="child" 
        />
      </ul>
    </li>
  </ul>
</template>

<script>
import Total from './Total.vue'

export default {
  name: 'Child',
  props: {
    model: {
      type: Object,
      default: {}
    },
    managers: {
      type: Array,
      default: []
    }
  },

  created() {
    this.auxiliar = this.managers
  },

  data() {
    return { 
      idDeveloper: 0,
      idQATester: 0,
      lastMove: 0,
      changes: 0,
      acumulate: 0,
      developersOpen : false,
      qa_testersOpen : false,
      childrenOpen : true,
      auxiliar: [],
      total: 1800
    }
  },

  components:{
    Total
  },

  computed: {
    calculateTotal(){
      let manager_id = this.model.id
      let descendants_ids = [...[manager_id], ...this.getDescendants([manager_id])]
      let descendants =  this.auxiliar.filter(manager => descendants_ids.includes(manager.id))

      let sum_developers = 0
      descendants.forEach(obj => {
        sum_developers += obj.developers
      })

      let sum_qa_testers = 0
      descendants.forEach(obj => {
        sum_qa_testers += obj.qa_testers
      })

      let total = (descendants.length * 300) + (sum_developers * 1000) + (sum_qa_testers * 500)
      return total
    },
  },

  methods: {

    addChild() {
      let id = this.auxiliar.length
      this.model.children.push({
        id: id,
        name: "Manager "+id, 
        developer: 1, 
        qa_tester: 1, 
        developers: [], 
        qa_testers: [], 
        children : [],
      })

      this.auxiliar.push({id: id, parent: this.model.id, developers: 0, qa_testers: 0})
    },

    addDeveloper() {
      this.idDeveloper++
      this.model.developers.push({
        id: this.idDeveloper,
        name: "Developer "+this.model.id+"."+this.idDeveloper
      })
      
      this.auxiliar[this.model.id].developers++
    },

    addQATester () {
      this.idQATester++
      this.model.qa_testers.push({
        id: this.idQATester,
        name: "QA Tester "+this.model.id+"."+this.idQATester
      })
      
      this.auxiliar[this.model.id].qa_testers++
    },

    getDescendants(ascendants) {
      let descendants = this.auxiliar.filter(manager => ascendants.includes(manager.parent)).map(m => m.id)
      if(descendants.length == 0) {
        return descendants
      }
      else return [...descendants, ...this.getDescendants(descendants)]
    },

    deleteManager() {
      let manager_id = this.model.id

      let result = this.$parent.model.children.filter(manager => manager.id != manager_id)
      this.$parent.model.children = result
 
      let descendants_ids = [...[manager_id], ...this.getDescendants([manager_id])]
      descendants_ids.forEach(id => {
        this.auxiliar[id] = {}
      });
    },

    deleteDeveloper(developer_id){
      let result = this.model.developers.filter(developer => developer.id != developer_id)
      this.model.developers = result
      this.auxiliar[this.model.id].developers--
    },

    deleteQATester(qa_tester_id){
      let result = this.model.qa_testers.filter(qa_tester => qa_tester.id != qa_tester_id)
      this.model.qa_testers = result
      this.auxiliar[this.model.id].qa_testers--
    },

    toggleChildren() {
      this.childrenOpen = !this.childrenOpen
    },

    toggleDevelopers() {
      this.developersOpen = !this.developersOpen
    },

    toggleQATesters() {
      this.qa_testersOpen = !this.qa_testersOpen
    },
  }
}
</script>

<style>
@import './styles.css';

body {
  overflow-x: scroll;
  background-color: rgb(255, 255, 255);
}

#general-information {
  margin: 0.75em, 0;
}

#list {
  margin-top: 0.5em;;
}

</style>
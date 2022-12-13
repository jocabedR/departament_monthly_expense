<template>

  <div>
    {{ model.name }}: $300.00
    <button @click="addChild" title="Add Manager">+👷</button>
    <button @click="addDeveloper" title="Add Developer">+👨‍💻</button>
    <button @click="addQATester" title="Add QA Tester">+🧐</button>
    <button @click="deleteManager(model.id)" title="Delete" v-if="model.id != 0">🗑️</button>
  </div>
  <ul>
    <li>
      <span @click="toggleDevelopers">[{{ developersOpen ? '-' : '+' }}] </span>
      <span>{{model.developers.length}} Developer(s)</span>
      <ul v-if="developersOpen">
        <li v-for="developer in model.developers">
          <span>{{developer.name}} : $1,000.00</span>
          <button @click="deleteDeveloper(developer.id)" title="Delete">🗑️</button>
        </li>
      </ul>
    </li>

    <li>
      <span @click="toggleQATesters">[{{ qa_testersOpen ? '-' : '+' }}] </span>
      <span>{{model.qa_testers.length}} QA Tester(s)</span>
      <ul v-if="qa_testersOpen">
        <li v-for="qa_tester in model.qa_testers">
          <span>{{qa_tester.name}} : $500.00</span>
          <button @click="deleteQATester(qa_tester.id)" title="Delete">🗑️</button>
        </li>
      </ul>
    </li>

    <li>
      <span @click="toggleChildren">[{{ childrenOpen ? '-' : '+' }}] </span>
      <span>{{model.children.length}} Manager(s)</span>
      <ul v-if="childrenOpen">
        <Child 
          v-for="child in model.children" 
          :model="child" 
          :managers="managers" 
          @totalChild="(change) => acumulate += change"
        />
      </ul>
    </li>

  <!-- <Total :total="calculateLocalTotal"/>
  <br/>
  <Total :total="this.model.total"/> -->
  <Total :total="calculateTotal(model.id)"/>

  <!-- <button @click="calculateTotal(model.id)">$</button> -->
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
      auxiliar: [] 
    }
  },

  components:{
    Total
  },

  computed: {
    calculateLocalTotal(){
      let totalChildren = 0

      for (let i = 1; i <= this.model.children.length; i++) {
        if(typeof this.model.children[i] != "undefined"){
          totalChildren += this.model.children[i].total
          console.log(totalChildren)
        } 
      } 

     return  (300 + (this.model.developers.length * 1000) + (this.model.qa_testers.length * 500))  + totalChildren
    },
    
  },

  methods: {

    addChild() {
      let id = this.managers.length
      this.model.children.push({
        id: id,
        name: "Manager", 
        developer: 1, 
        qa_tester: 1, 
        developers: [], 
        qa_testers: [], 
        children : []
      })

      this.managers.push({id: id, parent: this.model.id, developers: 0, qa_testers: 0})
    },

    addDeveloper() {
      this.idDeveloper++
      this.model.developers.push({
        id: this.idDeveloper,
        name: "Developer"
      })
      
      this.managers[this.model.id].developers++

      console.log(this.managers)

    },

    addQATester () {
      this.idQATester++
      this.model.qa_testers.push({
        id: this.idQATester,
        name: "QA Tester"
      })
      
      this.managers[this.model.id].qa_testers++
      console.log(this.managers)
    },

    getDescendants(ascendants) {
      let descendants = this.managers.filter(manager => ascendants.includes(manager.parent)).map(m => m.id)
      if(descendants.length == 0) {
        return descendants
      }
      else return [...descendants, ...this.getDescendants(descendants)]
    },

    deleteManager(manager_id){
      let result = this.$parent.model.children.filter(manager => manager.id != manager_id)
      this.$parent.model.children = result
 
      /* let descendants_ids = [...[manager_id], ...this.getDescendants([manager_id])]
      result = this.managers.filter(manager => !descendants_ids.includes(manager.id))
      "use strict"
      this.managers = result
      console.log(descendants_ids)
      console.log(this.managers)
      console.log(this.model) */
    },

    deleteDeveloper(developer_id){
      let result = this.model.developers.filter(developer => developer.id != developer_id)
      this.model.developers = result
      this.managers[this.model.id].developers--
    },

    deleteQATester(qa_tester_id){
      let result = this.model.qa_testers.filter(qa_tester => qa_tester.id != qa_tester_id)
      this.model.qa_testers = result
      this.model.total += -500
      this.managers[this.model.id].qa_testers--
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

    calculateTotal(manager_id){
      let descendants_ids = [...[manager_id], ...this.getDescendants([manager_id])]
      let descendants =  this.managers.filter(manager => descendants_ids.includes(manager.id))

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
  }
}
</script>

<style>
  ul{
    list-style-type: none;
  }

  button {
    margin: 0.25em;
  }
</style>
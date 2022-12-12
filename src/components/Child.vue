<template>
  <div>
    {{ model.name }}: $300.00
    <button @click="addChild">+M</button>
    <button @click="addDeveloper">+D</button>
    <button @click="addQATester">+QA</button>
    <button @click="deleteManager(model.id)">-</button>

  </div>
  <ul>
    <!-- <li v-for="developer in model.developers">
      <span>{{developer.name}} : $1,000.00</span>
      <button @click="deleteDeveloper(developer.id)">-</button>
    </li>
    <li v-for="qa_tester in model.qa_testers">
      <span>{{qa_tester.name}} : $500.00</span>
      <button @click="deleteQATester(qa_tester.id)">-</button>
    </li> -->

    <li>
      <span>{{model.developers.length}} Developer(s) : $1,000.00 each</span>
      <button @click="toggleDevelopers">[{{ developersOpen ? '-' : '+' }}]</button>
      <ul v-if="developersOpen">
        <li v-for="developer in model.developers">
          <span>{{developer.name}} : $1,000.00</span>
          <button @click="deleteDeveloper(developer.id)">-</button>
        </li>
      </ul>
    </li>
    <li>
      <span>{{model.qa_testers.length}} QA Tester(s) : $500.00 each</span>
      <button @click="toggleQATesters">[{{ qa_testersOpen ? '-' : '+' }}]</button>
      <ul v-if="qa_testersOpen">
        <li v-for="qa_tester in model.qa_testers">
          <span>{{qa_tester.name}} : $500.00</span>
          <button @click="deleteQATester(qa_tester.id)">-</button>
        </li>
      </ul>
    </li>
    
    <Child 
      v-for="child in model.children" 
      :model="child" 
      @updatedChild="(change) => acumulate += change"
    />

    <!-- <li v-if="model.children.length > 0" >
      Local:<Total :total="calculateLocalTotal"/>
      <br/>
      Changes:<Total :total="acumulate"/>
      <br/>
      TotalTotal <Total :total="acumulate + calculateLocalTotal"/>
    </li>
    <li v-else>
      <Total :total="calculateLocalTotal"/>
    </li> -->
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
    }
  },

  updated() {
    console.log(this.changes)
    this.$emit("updatedChild", this.changes)
  },

  data() {
    return { 
      id: 0,
      lastMove: 0,
      changes: 0,
      acumulate: 0,
      developersOpen : false,
      qa_testersOpen : false,
      childrenOpen : false,
    }
  },

  components:{
    Total
  },

  computed: {
    calculateLocalTotal(){
     return  (((this.model.children.length +1)*300) + (this.model.developers.length * 1000) + (this.model.qa_testers.length * 500))
    },

    isFolder() {
      return this.model.children && this.model.children.length
    }
    
  },

  methods: {
    getDescendants(ascendants) {
      let descendants = this.model.filter(manager => ascendants.includes(manager.parent)).map(m => m.id)
      if(descendants.length == 0) {
        return descendants
      }
      else return [...descendants, ...this.getDescendants(descendants)]
    },

    getChildren(parent_id) {
      let descendants = this.model.filter(manager => manager.parent == parent_id)
      return descendants
    },

    addChild() {
      this.id++
      this.model.children.push({
        id: this.id,
        name: "Manager", 
        developer: 1, 
        qa_tester: 1, 
        developers: [], 
        qa_testers: [], 
        children : []
      })
      this.changes = 300
    },

    addDeveloper() {
      this.id++
      this.model.developers.push({
        id: this.id,
        name: "Developer"
      })
      this.changes = 1000
    },

    addQATester () {
      this.id++
      this.model.qa_testers.push({
        id: this.id,
        name: "QA Tester"
      })
      this.changes = 500
    },

    deleteManager(manager_id){

      let result = this.$parent.model.children.filter(manager => manager.id != manager_id)
      this.$parent.model.children = result

      this.changes = -300

    },

    deleteDeveloper(developer_id){

      //console.log(developer_id)

      let result = this.model.developers.filter(developer => developer.id != developer_id)
      this.model.developers = result
      this.changes = -1000
    },

    deleteQATester(qa_tester_id){
      let result = this.model.qa_testers.filter(qa_tester => qa_tester.id != qa_tester_id)
      this.model.qa_testers = result
      this.changes = -500
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
.without {
 color: #f2f4f4 !important ;
}

.with {
  color: black;
}
</style>
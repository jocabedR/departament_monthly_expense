<template>
  <div>
    {{ model.name }}: $300.00
    <button @click="addChild">+M</button>
    <button @click="addDeveloper">+D</button>
    <button @click="addQATester">+QA</button>
    <button v-if="(model.id !=0 )" @click="deleteManager(model.id)">Delete manager</button>
  </div>
  <ul>
    <li v-for="developer in model.developers">
      <span>{{developer.name}} : $1,000.00</span>
      <button>-</button>
    </li>
    <li v-for="qa_tester in model.qa_testers">
      <span>{{qa_tester.name}} : $500.00</span>
    </li>
    
    <Child 
      v-for="child in model.children" 
      :model="child" 
      @updatedChild="(change) => changes += change"
    />

    <li v-if="model.children.length > 0" >
      <Total :total="changes"/>
    </li>
    <li v-else>
      <Total :total="calculateLocalTotal"/>
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
    }
  },

  updated() {
    console.log(this.changes)
    this.$emit("updatedChild", this.changes)
  },

  data() {
    return { 
      id: 0,
      idDeveloper: 0,
      idQaTester : 0,
      lastMove: 0,
      changes: 0,
      acumulateChanges: 0,
    }
  },

  components:{
    Total
  },

  computed: {

    calculateLocalTotal(){
     return  (300 + (this.model.developers.length * 1000) + (this.model.qa_testers.length * 500))
    },
    
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
        name: "Manager "+this.model.id+this.id, 
        developer: 1, 
        qa_tester: 1, 
        developers: [], 
        qa_testers: [], 
        children : []
      })
      this.changes += 300
    },

    addDeveloper() {
      this.idDeveloper++
      this.model.developers.push({
        name: "Developer"
      })
      this.changes += 1000
    },

    addQATester () {
      this.idQaTester++
      this.model.qa_testers.push({
        name: "QA Tester"
      })
      this.changes += 500
    },

    deleteManager(manager_id){
      let descendants_ids = [...[manager_id], ...this.getDescendants([manager_id])]
      console.log(descendants_ids)
      let result = this.model.filter(manager => !descendants_ids.includes(manager.id))
      this.model = result
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
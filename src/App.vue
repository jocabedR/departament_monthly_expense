<template>
  <h1>Departament's monthly expense</h1>
  <ul>
    <li v-for="manager in managers" :key="manager.id">
      <span>{{ manager.name }}: ${{manager_cost}}</span>
      <button @click="addChild(manager.id)">+</button>
      <ul>
        <li>
          <span :class="manager.devloper ? 'with' : 'witout'">Developver: ${{developver_cost}}</span>
          <input type="checkbox" v-model="manager.devloper">
        </li>
        <li>
          <span :class="manager.devloper ? 'with' : 'witout'">QA Tester: ${{qa_tester_cost}}</span>
          <input type="checkbox" v-model="manager.qa_tester">
        </li>
        <li>
          <TotalVue :total="cualculaTotal(manager.id)"/>
        </li>
      </ul>
    </li>
  </ul>
  
</template>

<script>
import TotalVue from './components/Total.vue'

export default {
  data() {
    return {
      id: 0,
      managers: [
        {id: 0, parent: null, name: "Manager 0", devloper: true, qa_tester: true },
      ],
      parents: [],
      manager_cost : 300.00,
      developver_cost : 1000.00,
      qa_tester_cost : 500.00
    }
  },
  
  name: 'App',
  components: {
    TotalVue
  },
  computed: {
    
  },
  methods: {
    //It's going to be used to find total per manager
    getDescendants(ascendants) {
      let descendants = this.managers.filter(manager => ascendants.includes(manager.parent)).map(m => m.id)
      if(descendants.length == 0) {
        return descendants
      }
      else return [...descendants, ...this.getDescendants(descendants)]
    },

    cualculaTotal(manager_id){
      let descendants_ids = [...[manager_id], ...this.getDescendants([manager_id])]

      let descendants =  this.managers.filter(manager => descendants_ids.includes(manager.id))
      let developvers = descendants.filter(manager => manager.devloper)
      let qa_testers = descendants.filter(manager => manager.qa_tester)

      let total = (descendants.length * this.manager_cost) + (developvers.length * this.developver_cost) + (qa_testers.length * this.qa_tester_cost)
      return total
    },

    addChild(parent_id) {
      this.id ++
      this.managers.push({id: this.id, parent: parent_id, name: "Manager "+this.id, devloper: true, qa_tester: true})
    },
    
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: black;
  margin-top: 60px;
}

.without {
 color: #f2f4f4 !important ;
}

.with {
  color: black;
}
</style>
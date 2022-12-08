<template>
  <h1>Departament's monthly expense</h1>
  <ul>
    <li v-for="manager in managers" :key="manager.id">
      <span>{{ manager.name }}  </span>
      <button @click="aaalert([manager.id])">children</button>
      <button @click="addChild(manager.id)">+</button>
    </li>
  </ul>

  
</template>

<script>
//let id = 1
/* const manager_cost = 300
const developver_cost = 100
const qa_tester_cost = 5000 */
export default {
  data() {
    return {
      id: 1,
      managers: [
        {id: 1, parent: null, name: "Manager A", devloper: true, qa_tester: true },
      ],
      parents: []
    }
  },
  
  name: 'App',
  components: {
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

    // Just to che
    aaalert(ascendants){
      console.log(this.getDescendants(ascendants))
    },


    addChild(parent_id) {
      this.id ++
      this.managers.push({id: this.id, parent: parent_id, name: "Manager", devloper: true, qa_tester: true})
      //console.log(this.managers)
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
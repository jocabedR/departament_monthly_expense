<template>
  <h1>Departament's monthly expense</h1>
  <ul>
    <button>+</button>
    <li v-for="manager in managers" :key="manager.id">
      <!-- <input type="checkbox" v-model="manager.done"> -->
      <span >{{ manager.name }} </span>
      <button>X </button>
    </li>
  </ul>

  
</template>

<script>
var id = 0
export default {

  data() {
    return {
      
      managers: [
        {parent: null, id: id, name: "Manager A", devloper: true, qa_tester: true },
        {parent: id, id: id++, name: "Manager B", devloper: true, qa_tester: true },
      ],
    }
  },
  
  name: 'App',
  components: {
  },

  computed: {
    filteredmanagers() {
      return this.hideCompleted
        ? this.managers.filter((t) => !t.done)
        : this.managers
    }
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  margin-top: 60px;
}
</style>

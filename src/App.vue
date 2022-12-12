<template>
  <h1>Departament's monthly expense</h1>
  <Child :model="model"/>  
</template>

<script>
import Child from './components/Child.vue'

export default {
  data() {
    return {
      model:
      {
        id: 0,
        name: "Manager 0",
        developers: [
          {id:0, name: "JocaDev"}
        ],
        qa_testers: [
          {id:0, name: "JocaTest"}
        ],
        children : []
      },
    }
  },
  
  name: 'App',
  components: {
    Child
  },
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
</style>
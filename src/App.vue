<template>
  <h1>Departament's monthly expense</h1>
  <Child :model="model" :managers="managers"/>
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
        children : [],
        total: 1800
      },
      managers: [
        {id: 0, 
        parent: null, 
        developers: 1, 
        qa_testers: 1},
      ],
    }
  },
  
  name: 'App',
  components: {
    Child
  },
}
</script>

<style>
h1 {
  color: rgb(53, 154, 223);
}

#app {
  font-family: "Monospace";
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: rgb(255, 255, 255);
  margin: 5em;
}

html, 
body {
    margin: 0;
    padding: 0;
    background-color:rgb(0, 0, 0);
}
</style>
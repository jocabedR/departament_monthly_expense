<template>
  <h1 id="title">Departament's monthly expense</h1>
  <Child  class="init" :model="model" :managers="managers" id="child"/>
  <img class="absolute" v-bind:src="require('./assets/astronaut.png')" alt="Astronaut 1" id="img1"/>
  <img class="absolute" v-bind:src="require('./assets/astronaut(1).png')" alt="Astronaut 2" id="img2"/>
  <img class="absolute" v-bind:src="require('./assets/planets.png')" alt="Planets" id="img3"/>
  <img class="absolute" v-bind:src="require('./assets/planets.png')" alt="Planets" id="img4"/>
  <img class="absolute" v-bind:src="require('./assets/planets.png')" alt="Planets" id="img5"/>
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
@import './components/styles.css';

#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: rgb(255, 255, 255);
  background-color: rgba(0,0,0,0) ;
  padding: 3em;
  margin-bottom: 35em;
}

body{
  background: rgb(0,0,0);
  background: linear-gradient(180deg, rgba(0,0,0,1) 0%, rgba(18,46,84,1) 100%);
}

h1 {
  position: absolute;
  z-index: 100;
}

.init {
  position: absolute;
  z-index: 100;
  left: 3em;
  top: 7em;
  max-height: 30em;
  overflow-y: scroll;
  max-width: 60em;
  overflow-x: scroll;
}

#img1 {
  position: absolute;
  z-index: 0;
  left: 3em;
  bottom: 2em;
  width: 10em;
}

#img2 {
  transform: scaleX(-1);
  width: 18em;
  position: absolute;
  right: 1em;
  top: 0.5em;
  padding: 5em;
  z-index: 0;
}

#img3 {
  width: 2em;
  position: absolute;
  right: 25em;
  bottom: 5em;
  z-index: 0;
}

#img4 {
  transform: scaleX(-1);
  width: 2em;
  position: absolute;
  left: 20em;
  top: 15em;
  z-index: 0;
}

#img5 {
  transform: scaleY(-1);
  position: absolute;
  width: 2em;
  left: 50%;
  top: 50%;
  z-index: 0;
}

</style>
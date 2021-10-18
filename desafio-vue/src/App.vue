<template>
  <div id="main-container">
    <NavBar/>
    
    <div class="container">
        <FormSimulacao v-if="!makedSimulation" @makedSimulation="openDashboardSimulation"/>
        <Dashboard :dataSimulation="dataSimulation" @closeDashboard="openFormSimulation" v-else/>
    </div>
    <Footer />
  </div>
</template>

<script>
import M from 'materialize-css'
import NavBar from "./components/Navbar.vue";
import FormSimulacao from "./components/FormSimulacao.vue";
import Dashboard from "./components/Dashboard.vue";
import Footer from "./components/Footer.vue";


export default {
  name: 'App',
  components: {
    NavBar,
    FormSimulacao,
    Dashboard,
    Footer
  },
  mounted() {
    M.AutoInit()
  },
  data() {
    return {
      makedSimulation: false,
      dataSimulation: null
    }
  },
  methods: {
    openDashboardSimulation(data) {
      this.dataSimulation = data 
      
      this.makedSimulation = true
      
    },
    openFormSimulation() {
      this.makedSimulation = false
      
    }
  }
}
</script>

<style>
  #app {
    background-color: #eceff1;
    min-width: 350px;
  }
</style>

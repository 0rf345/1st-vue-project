<template>
  <div id="app">
    <navbar :back="createBackButtonProp" @homeSignal="homePressed()" @backSignal="backPressed"></navbar>
    <home @createBackButton="backButtonCreate" :justHome="cameHome" :justBack="cameBack" @gotIt="cameBack=cameHome = false"></home>
    <projectFooter></projectFooter>
  </div>
</template>

<script>
import navbar from './components/Navbar'
import projectFooter from './components/ProjectFooter'
import home from './components/Home'

export default {
  name: 'App',
  data () {
    return {
      createBackButtonProp: false,
      cameBack: false,
      cameHome: false
    }
  },
  components: {
    navbar,
    home,
    projectFooter
  },
  methods: {
    backButtonCreate: function () {
      this.createBackButtonProp = true
    },
    backPressed: function () {
      this.createBackButtonProp = false
      this.cameBack = true
    },
    homePressed: function () {
      console.log('mpla')
      this.createBackButtonProp = false
      this.cameHome = true
      this.cameBack = true
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

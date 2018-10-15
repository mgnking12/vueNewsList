<template>
  <div class="container" id="app">
    <SaatvaNews v-on:sourceChanged="sourceChanged"></SaatvaNews>
  </div>
</template>

<script>
// import Newslist from './components/Newslist'
import SaatvaNews from './components/SaatvaNews'

export default {
  name: 'app',
  components: {
    // Newslist,
    SaatvaNews
  },
  data () {
    return {
      source: ""
    }
  },
  methods: {
    sourceChanged: function (source) {
      if(this.sourceChanged === source){
        this.source = source;
      }
      
    }
  }
}
</script>

<style lang="scss">
@import './styles/custom-styling.scss';

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>


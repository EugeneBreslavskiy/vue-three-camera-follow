<template>
  <div id="app">
    <a href="https://unsplash.com/@osmanrana">Osman Rana</a>
    <Scene/>
  </div>
</template>

<script>
import Scene from './components/Scene'

export default {
  name: 'App',
  components: {
    Scene
  }
}
</script>

<style lang="sass">
  
  @import url('https://fonts.googleapis.com/css?family=Titillium+Web:300&display=swap')
  
  body
    margin: 0
    font-family: 'Titillium Web', sans-serif
    overflow: hidden
  
  a
    font-size: 12px
    color: #1a1a1a
    text-decoration: none
    position: absolute
    top: 20px
    left: 20px
    z-index: 10

</style>

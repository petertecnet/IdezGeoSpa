<template>
  <div id="app">
    <IdezGeoInfo /> <!-- Adicione esta linha -->
   
  </div>
</template>

<script>
import IdezGeoInfo from './components/IdezGeoInfo.vue'; 

export default {
  name: 'App',
  components: {
    IdezGeoInfo 
  }
}
</script>

<style>
/* ... */
</style>

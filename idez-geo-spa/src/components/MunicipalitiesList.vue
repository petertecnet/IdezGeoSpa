<template>
  <div>
    <h1>Lista de Municípios</h1>
    <ul v-if="municipios.length > 0">
      <li v-for="municipio in municipios" :key="municipio.ibge_code">
        {{ municipio.name }}
      </li>
    </ul>
    <div v-else>
      <p>Carregando...</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      municipios: [],
    };
  },
  mounted() {
    const uf = 'RS'; // Substitua pela UF desejada
    axios.get(`http://localhost:8000/api/municipalities/${uf}`)
      .then(response => {
        this.municipios = response.data.municipalities;
      })
      .catch(error => {
        console.error('Erro ao obter municípios:', error);
      });
  },
};
</script>

<style scoped>
/* Estilos específicos para este componente, se necessário */
</style>

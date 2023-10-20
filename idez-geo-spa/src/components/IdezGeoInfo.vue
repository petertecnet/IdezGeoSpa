<template>
  <div class="idez-geo-info">
    <div class="intro">
      <h1>Bem-vindo ao IdezGeo</h1>
      <img src="https://media.licdn.com/dms/image/D4D0BAQGZrvjRiJZ1LA/company-logo_200_200/0/1697479325216?e=1706140800&v=beta&t=tn5ISQWUQ-qz0hDf99aNFIddKJ7Kf8TJnw6oUYuEwtw" alt="Idez Logo" />
      <p>
        O IdezGeo é uma aplicação que permite explorar informações geográficas de municípios brasileiros.
      </p>
    </div>
    <div class="uf-buttons">
      <button
        v-for="uf in ufs"
        :key="uf"
        @click="fetchMunicipalities(uf)"
      >
        {{ uf }}
      </button>
    </div>

    <div v-if="loading" class="loading-circle">
      <div class="spinner"></div>
    </div>

    <div v-if="municipalities.length > 0" class="municipality-list">
      <div
        v-for="municipality in municipalities"
        :key="municipality.ibge_code"
        class="municipality-card"
      >
        <div class="municipality-name">{{ municipality.name }}</div>
        <div class="municipality-ibge">IBGE: {{ municipality.ibge_code }}</div>
      </div>
    </div>

    <div v-if="totalPages > 1" class="pagination">
      <span>Página {{ currentPage }} de {{ totalPages }}</span>
      <button :disabled="currentPage === 1" @click="prevPage">Anterior</button>
      <button :disabled="currentPage === totalPages" @click="nextPage">Próxima</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      ufs: ['AC', 'AL', 'AM', 'AP', 'BA', 'CE', 'DF', 'ES', 'GO', 'MA', 'MG', 'MS', 'MT', 'PA', 'PB', 'PE', 'PI', 'PR', 'RJ', 'RN', 'RO', 'RR', 'RS', 'SC', 'SE', 'SP', 'TO'],
      loading: false,
      municipalities: [],
      currentPage: 1,
      totalPages: 1,
      uf: null,
    };
  },
  methods: {
    async fetchMunicipalities(uf) {
      this.loading = true;
      this.municipalities = [];
      this.uf = uf;

      const apiUrl = `http://localhost:8000/api/municipalities/${uf}?page=${this.currentPage}`; // Adicionando a página à URL

      try {
        const response = await fetch(apiUrl);
        const data = await response.json();

        this.loading = false;
        this.municipalities = data.municipalities;
        this.currentPage = data.current_page;
        this.totalPages = Math.ceil(data.total / data.per_page);
      } catch (error) {
        console.error('Erro na requisição:', error);
        this.loading = false;
      }
    },
    prevPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
        this.fetchMunicipalities(this.uf);
      }
    },
    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++;
        this.fetchMunicipalities(this.uf);
      }
    },
  },
};
</script>

<style scoped>
.intro {
  text-align: center;
  margin-top: 50px;
  transition: transform 0.3s ease, opacity 0.3s ease, filter 0.3s ease;
}

.intro img {
  max-width: 100px;
  max-height: 100px;
}

.intro p {
  font-size: 18px;
}

.intro:hover {
  transform: scale(1.05);
  opacity: 0.9;
  filter: brightness(120%);
}
.uf-buttons {
  display: flex;
  flex-wrap: wrap;
}

.uf-buttons button {
  margin: 5px;
  padding: 10px 20px;
  font-size: 16px;
  background-color: #3498db;
  color: #fff;
  border: none;
  cursor: pointer;
  border-radius: 5px;
}

.loading-circle {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100px;
}

.spinner {
  border: 4px solid rgba(255, 255, 255, 0.3);
  border-top: 4px solid #3498db;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

.municipality-list {
  display: flex;
  flex-wrap: wrap;
}

.municipality-card {
  width: 200px;
  padding: 10px;
  margin: 10px;
  background: linear-gradient(to right, #4a69bd, #72c6ef);
  border-radius: 10px;
  cursor: pointer;
  transition: transform 0.3s ease, opacity 0.3s ease;
}

.municipality-card:hover {
  transform: scale(1.05);
  opacity: 0.9;
}

.municipality-name {
  font-size: 18px;
  color: #fff;
}

.municipality-ibge {
  font-size: 14px;
  color: #fff;
}

.pagination {
  display: flex;
  align-items: center;
  margin-top: 10px;
}

.pagination span {
  margin-right: 10px;
  font-size: 16px;
}

.pagination button {
  padding: 5px 10px;
  font-size: 14px;
  background-color: #3498db;
  color: #fff;
  border: none;
  cursor: pointer;
  border-radius: 5px;
  margin-right: 5px;
}

.pagination button:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}
</style>

<template>
  <div class="container">
    <div class="onpage">
      <h1>Doorzoek de Galerij van het Rijksmuseum</h1>
      <form @submit.prevent="search" class="search-form">
        <input type="text" placeholder="Titel" v-model="searchByTitle" />
        <input type="text" placeholder="Maker" v-model="searchByMaker" />
        <select v-model="searchByPeriod">
          <option value="">Kies een periode</option>
          <option value="1">0 - 99 (1e eeuw)</option>
          <option value="2">100 - 199 (2e eeuw)</option>
          <option value="3">200 - 299 (3e eeuw)</option>
          <option value="4">300 - 399 (4e eeuw)</option>
          <option value="5">400 - 499 (5e eeuw)</option>
          <option value="6">500 - 599 (6e eeuw)</option>
          <option value="7">600 - 699 (7e eeuw)</option>
          <option value="8">700 - 799 (8e eeuw)</option>
          <option value="9">800 - 899 (9e eeuw)</option>
          <option value="10">900 - 999 (10e eeuw)</option>
          <option value="11">1000 - 1099 (11e eeuw)</option>
          <option value="12">1100 - 1199 (12e eeuw)</option>
          <option value="13">1200 - 1299 (13e eeuw)</option>
          <option value="14">1300 - 1399 (14e eeuw)</option>
          <option value="15">1400 - 1499 (15e eeuw)</option>
          <option value="16">1500 - 1599 (16e eeuw)</option>
          <option value="17">1600 - 1699 (17e eeuw)</option>
          <option value="18">1700 - 1799 (18e eeuw)</option>
          <option value="19">1800 - 1899 (19e eeuw)</option>
          <option value="20">1900 - 1999 (20e eeuw)</option>
          <option value="21">2000 - 2099 (21e eeuw)</option>
        </select>
        <select v-model="sortBy">
          <option value="">Sorteer op</option>
          <option value="relevance">Relevantie</option>
          <option value="objecttype">Objecttype</option>
          <option value="chronologic">Chronologisch</option>
          <option value="achronologic">Achronologisch</option>
          <option value="artist">Kunstenaar</option>
          <option value="artistdesc">Kunstenaar (omgekeerd)</option>
        </select>
        <button type="submit">Zoeken</button>
      </form>
      <div id="results" class="results-container">
        <div v-for="artObject in artObjects" :key="artObject.id" class="art-object">
          <h2>{{ artObject.title }}</h2>
          <a :href="artObject.webImage.url" target="_blank">
            <img :src="artObject.webImage.url" :alt="artObject.title" />
          </a>
          <p>{{ artObject.objectNumber }}</p>
          <p>{{ artObject.longTitle }}</p>
          <p>{{ artObject.principalOrFirstMaker }}</p>
          <a :href="artObject.links.web" target="_blank">Bekijk op de website van het Rijksmuseum</a>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      artObjects: [],
      searchByTitle: '',
      searchByMaker: '',
      searchByPeriod: '',
      sortBy: '',
    };
  },
  methods: {
    search() {
      const url = `https://www.rijksmuseum.nl/api/nl/collection?key=5GjkkqBX&involvedMaker=${this.searchByMaker}&title=${this.searchByTitle}&f.dating.period=${this.searchByPeriod}&s=${this.sortBy}`;
      fetch(url)
        .then((response) => response.json())
        .then((data) => {
          console.log(data);
          this.artObjects = data.artObjects;
        })
        .catch((error) => {
          console.error(error);
          this.artObjects = [];
        });
    },
  },
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Ruda:wght@400;700&display=swap');

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

.container {
  margin: 0 auto;
  padding: 2rem;
  background-image: url('../assets/bg.jpg');
}

h1 {
  text-align: center;
  font-family: 'Ruda', sans-serif;
  font-size: 2rem;
  margin-bottom: 2rem;
  color: #fff;
}

.onpage {
  background-color: rgba(0, 0, 0, 0.777); 
  padding: 2rem;
  border-radius: 5px;
  min-height: 650px;
}

.search-form {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  margin-bottom: 2rem;
}

.search-form input[type="text"],
.search-form select {
  flex: 1;
  padding: 1rem;
  margin-right: 1rem;
  margin-bottom: 1rem;
  border: 1px solid #000;
  border-radius: 5px;
  font-family: 'Ruda', sans-serif;
  font-size: 1rem;
}

.search-form button[type="submit"] {
  padding: 1rem 2rem;
  border: none;
  border-radius: 5px;
  font-family: 'Ruda', sans-serif;
  font-size: 1rem;
  background-color: #000;
  color: #fff;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.search-form button[type="submit"]:hover {
  background-color: #555;
}

.results-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.art-object {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 300px;
  margin: 1rem;
  padding: 2rem;
  background-color: #fff;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  border-radius: 5px;
  transition: transform 0.3s ease;
}

.art-object:hover {
  transform: translateY(-5px);
}

.art-object img {
  width: 100%;
  height: auto;
  border: #000 2px solid;
  transition: transform 0.3s ease;
}

.art-object:hover img {
  transform: scale(1.05);
}

.art-object h2 {
  text-align: center;
  font-family: 'Ruda', sans-serif;
  font-size: 1rem;
  margin-top: 1rem;
  margin-bottom: 0.5rem;
}

.art-object p {
  text-align: center;
  font-family: 'Ruda', sans-serif;
  font-size: 0.8rem;
  margin: 0.5rem;
}

.art-object a {
  text-align: center;
  font-family: 'Ruda', sans-serif;
  font-size: 0.8rem;
  color: #000;
  text-decoration: none;
  transition: color 0.3s ease;
}

.art-object a:hover {
  color: #555;
}

@media (max-width: 768px) {
  .container {
    padding: 1rem;
  }

  h1 {
    font-size: 1.5rem;
    margin-bottom: 1rem;
  }

  .search-form {
    flex-direction: column;
  }

  .search-form input[type="text"],
  .search-form select {
    flex: auto;
    margin-right: 0;
  }

  .search-form button[type="submit"] {
    flex: none;
    margin-top: 1rem;
  }

  .art-object {
    width: 100%;
    margin: 0.5rem;
    padding: 1rem;
  }
}
</style>

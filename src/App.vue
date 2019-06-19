<template>
  <div id="app">
    <div class="container">
      <header>
        <h1 class='title'>iTunes Catalog</h1>
        <span class="show-fav" v-on:click="showFav">My Favorites</span>
      </header>

      <input v-model="term" placeholder="Search..." v-on:keyup.enter="getData" >

      <div v-if="error" class="error">
        {{ error }}
      </div>

      <div v-if="loading" class="results">
        loading...
      </div>

      <Results v-else
        :items="items"
        :favs="favs"
        :toggleFav="toggleFav"
      />
      <Favorites :favs="favs" :removeFav="removeFav" />
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import Results from './components/Results.vue'
import Favorites from './components/Favorites.vue'

export default {
  name: 'app',
  data() {
    return {
      favs: JSON.parse(window.localStorage.getItem('itunesFav')) || [],
      term: '',
      loading: false,
      items: {},
      error: '',
    }
  },
  components: {
    Results,
    Favorites
  },
  methods: {
    // Fetches results when the component is created.
    getData() {
      this.loading = true;
      this.errors = null;
      axios.get(`https://thawing-falls-75528.herokuapp.com/api/${this.term}`, {
      })
      .then(response => {
        // JSON responses are automatically parsed.
        this.items = response.data;
        this.loading = false;
      })
      .catch(e => {
        this.error = e;
      })
    },
    showFav() {
      document.querySelector('.fav').style.right = '0';
    },
    toggleFav(event, id, name, url, artwork, genre) {
      let element = event.target;
      element = element.closest('.love');
      element.classList.toggle('on');

      if(element.classList.contains('on')) {
        // add to favortes
        let item = {
          id,
          name,
          url,
          artwork,
          genre,
        };

        this.favs.push(item);
        window.localStorage.setItem('itunesFav', JSON.stringify(this.favs));

      } else {
        // remove from favorites
        this.removeFav(id);
      }
    },

    removeFav(id) {
      // remove from favorites
      const index = this.favs.findIndex(item => item.id == id);
      this.favs.splice(index, 1);
      window.localStorage.setItem('itunesFav', JSON.stringify(this.favs));

      // unlove in the results
      const element = document.querySelector(`#id-${id}`);
      if(element) {
        for(let i = 0; i < element.childNodes.length; i++) {
          const currNode = element.childNodes[i];
          if(currNode.classList.contains('love')) {
            currNode.classList.remove('on');
            break;
          }
        }
      }
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
}
.container {
  max-width: 1200px;
  margin: 0 auto;
}

a {
  text-decoration: none;
}

header {
  height: 60px;
  text-align: center;
  position: relative;
}

.title {
  font-size: 20px;
}

.show-fav {
  position: absolute;
  right: 10px;
  top: 0px;
  cursor: pointer;
}

input {
  padding: 10px;
  border: 0;
  border-bottom: 2px solid #333;
  width: 100%;
  font-size: 20px;
}

</style>

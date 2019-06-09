<template>
  <div>
    <div class="loading" v-if="loading">
      Loading...
    </div>

    <div v-if="error" class="error">
      {{ error }}
    </div>

    <section class="results">
      <div class="results__kind" v-for="kind in Object.keys(items)" :key="kind">
        <h2>{{kind}}</h2>
        <ul class="list">
          <li v-for="item in items[kind]" class="list__item" :key="item.id">
            <!-- <router-link :to="{name: 'movie', params: {id: movie._id}}"> -->
              <img v-if="item.artwork" :src="item.artwork"/>
              <div>
                <a href="item.url">
                  <h3>{{item.name}}</h3>
                </a>
              </div>
            <!-- </router-link> -->
          </li>
        </ul>
      </div>

    </section>

  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: "Movies",
  data() {
    return {
      loading: true,
      items: {},
      error: '',
    }
  },
  created() {
    this.getData();
  },
  methods: {
    // Fetches results when the component is created.
    getData() {
      this.loading = true;
      this.errors = null;
      axios.get(`http://thawing-falls-75528.herokuapp.com/api/frozen`)
      .then(response => {
        // JSON responses are automatically parsed.
        this.items = response.data
        this.loading = false;
      })
      .catch(e => {
        this.error = e;
      })
    }
  }
};

</script>

<style scoped>
.list {
  margin: 1rem;
}

.movies-list__directed-by {
  display: block;
  font-size: 1rem;
}
</style>

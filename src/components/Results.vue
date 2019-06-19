<template>
  <div>
    <section class="results">
      <div v-if="nothingFound">
        Nothing found.
      </div>
      <div v-else class="results__kind" v-for="kind in Object.keys(items)" :key="kind">
        <h2 class="kind">{{kind}}</h2>
        <ul class="list">
          <li :id="'id-' + item.id" v-for="item in items[kind]" class="list__item" :key="item.id">
            <div>
              <a class="item-url" :href="item.url" target="_blank">
                <h3 class="item-name">{{item.name}}</h3>
              </a>
            </div>
            <img class="item-artwork" v-if="item.artwork" :src="item.artwork"/>
            <div>
              Genre: <span class="item-genre">{{item.genre}}</span>
            </div>
            <span :class="isFav(item.id) ? 'love on' : 'love'" @click="toggleFav($event, item.id, item.name, item.url, item.artwork, item.genre)" >
              <Icon name="heart"/>
            </span>
          </li>
        </ul>
      </div>

    </section>

  </div>
</template>

<script>
import 'vue-awesome/icons';
import Icon from 'vue-awesome/components/Icon';

export default {
  name: "Movies",
  components: {
    Icon,
  },
  props: {
    toggleFav: Function,
    items: Object,
    favs: Array,
    nothingFound: Boolean,
  },
  methods: {
    isFav(id) {
      const index = this.favs.findIndex(item => item.id == id);
      return index > -1;
    }
  }
};

</script>

<style scoped>
.list {
  margin: 1rem;
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  grid-column-gap: 15px;
  grid-row-gap: 15px;
  justify-items: center;
  list-style: none;
  padding: 0;
}

.kind {
  margin-top: 50px;
}

.list__item {
  width: 200px;
  box-shadow: 0px 0px 30px 0px #ccc;
  margin: 5px;
  padding: 10px;
}

.movies-list__directed-by {
  display: block;
  font-size: 1rem;
}

.love {
  text-decoration: underline;
  cursor: pointer;
}
.love.on {
  color: red;
}
</style>

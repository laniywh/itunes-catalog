<template>
  <div>
    <section class="results">
      <div class="results__kind" v-for="kind in Object.keys(items)" :key="kind">
        <h2>{{kind}}</h2>
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
            <span :class="isFav(item.id) ? 'love on' : 'love'" @click="toggleFav($event, item.id, item.name, item.url, item.artwork, item.genre)">Love</span>
          </li>
        </ul>
      </div>

    </section>

  </div>
</template>

<script>

export default {
  name: "Movies",
  props: {
    toggleFav: Function,
    items: Object,
    favs: Array,
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
  display: flex;
  flex-wrap: wrap;
  list-style: none;
  padding: 0;
}

.list__item {
  width: 200px;
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

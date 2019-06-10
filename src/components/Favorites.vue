<template>
  <div class="fav">
    <h2>My Favorites</h2>
    <span class="fav__close" @click="hideFav">X</span>
    <ul class="fav-list">
      <li v-for="item in favs" class="fav-list__item" :key="item.id">
        <div>
          <a class="item-url" :href="item.url" target="_blank">
            <h3 class="item-name">{{item.name}}</h3>
          </a>
        </div>
        <img class="item-artwork" v-if="item.artwork" :src="item.artwork"/>
        <div>
          Genre: <span class="item-genre">{{item.genre}}</span>
        </div>
        <span class="fav-list__remove" @click="removeFav(item.id)">Remove</span>
      </li>
    </ul>

  </div>
</template>

<script>
export default {
  name: "Favorites",
  props: {
    favs: Array,
    removeFav: Function,
  },
  methods: {
    hideFav() {
      document.querySelector('.fav').style.right = '-500px';
    }
  }
}

</script>

<style scoped>
.fav {
  width: 300px;
  height: 100vh;
  position: fixed;
  top: 0;
  right: -500px;
  background: #999;
  overflow: scroll;
}
.fav__close {
  position: absolute;
  top: 10px;
  right: 10px;
  padding: 5px;
}
.fav-list {
  list-style: none;
  padding: 0;
}
.fav-list__remove {
  cursor: pointer;
  color: red;
}
</style>

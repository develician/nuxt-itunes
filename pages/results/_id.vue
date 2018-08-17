<template>
  <div>
    <div class="title">Itunes Album of {{$route.params.id}}</div>
    <div v-if="albumExists">
      <div class="card-wrapper">
        <div v-for="(album, index) in albumData" v-bind:key="index" class="inner">
          <Card class="card" :title="album.collectionCensoredName" :image="album.artworkUrl100" :artistName="album.artistName" :url="album.artistViewUrl" :color="picker(index)" />
        </div>
      </div>
    </div>
    <div v-else>
      <h1>Could not find album</h1>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Card from "~/components/Card.vue";

export default {
  asyncData({ params }) {
    return axios
      .get(`https://itunes.apple.com/search?term=${params.id}&entity=album`)
      .then(res => {
        return {
          albumData: res.data.results
        };
      });
  },
  middleware: "search",
  components: {
    Card
  },
  computed: {
    albumExists() {
      return this.albumData.length > 0;
    }
  },
  methods: {
    picker(index) {
      return index % 2 == 0 ? "red" : "blue";
    }
  }
};
</script>

<style lang="scss" scoped>
.title {
  display: flex;
  justify-content: center;
  padding: 1rem;
  font-weight: 600;
}

.card-wrapper {
  display: flex;
  flex-direction: column;

  .inner {
    width: 100%;
    display: flex;
    .card {
      width: 100%;
      flex: 1;
    }
  }
}
</style>
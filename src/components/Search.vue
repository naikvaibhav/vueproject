<template>
  <div class="search">
    <h3>Type in you search term</h3>
    <form v-on:submit.prevent="getImages(query)">
      <input type="text" placeholder="Search your term" v-model="query" />
    </form>
    <div v-if="results">
      <div>
        <div v-bind:key="result" v-for="result in results">
          <img v-bind:src="result.links[0].href" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Search",
  props: {
    // msg: String
  },
  data() {
    return {
      query: "",
      results: " "
    };
  },
  methods: {
    getImages(query) {
      axios
        .get(
          "https://images-api.nasa.gov/search?q=" + query + "&media_type=image"
        )
        .then(res => {
          this.results = res.data.collection.items;
        });
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>

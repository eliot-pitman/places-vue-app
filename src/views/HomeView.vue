<script>
import axios from "axios";

export default {
  data: function () {
    return { places: {}, post: {} };
  },
  created: function () {
    axios.get("/places").then((response) => {
      this.places = response.data;
      console.log("all places", this.places);
    });
  },
  methods: {
    placesShow: function () {
      axios.get("/places/" + this.$route.params.id).then((response) => {
        console.log(response.data);
        this.post = response.data;
      });
    },
  },
};
</script>

<template>
  <div class="home">
    <div v-for="place in places" :key="place.id">
      <h1>{{ place.name }}</h1>
      <h2>{{ place.address }}</h2>
    </div>
  </div>
</template>

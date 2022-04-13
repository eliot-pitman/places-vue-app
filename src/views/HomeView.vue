<script>
import axios from "axios";

export default {
  data: function () {
    return { places: {}, post: {}, currentPlace: {}, newPlacesParams: {}, errors: [] };
  },
  created: function () {
    axios.get("/places").then((response) => {
      this.places = response.data;
      console.log("all places", this.places);
    });
  },
  methods: {
    placesShow: function (place) {
      console.log(place);
      this.currentPlace = place;
      document.querySelector("#Place-details").showModal();
    },
    placesUpdate: function (place) {
      var placeParams = place;
      axios
        .patch("/places/" + place.id, placeParams)
        .then((response) => {
          console.log("success", response.data);
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
    placesDestroy: function (place) {
      axios
        .delete("/places/" + place.id)
        .then((response) => {
          console.log("successfully deleted", response.data);
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
    placesCreate: function () {
      axios
        .post("/places", this.newPlacesParams)
        .then((response) => {
          console.log("success", response.data);
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>create a new contact</h1>
    <div id="places-new">
      <h2>name:</h2>
      <input type="text" v-model="newPlacesParams.name" />
      <br />
      <h2>address:</h2>
      <input type="text" v-model="newPlacesParams.address" />
      <br />
      <button @click="placesCreate">create</button>
      <ul>
        <li v-for="error in errors" :key="error.id">{{ error }}</li>
      </ul>
    </div>
    <div v-for="place in places" :key="place.id">
      <h1>{{ place.name }}</h1>
      <h2>{{ place.address }}</h2>
      <button id="info-button" @click="placesShow(place)">click for more info</button>
      <ul>
        <li v-for="error in errors" :key="error.id">{{ error }}</li>
      </ul>
    </div>
    <dialog id="Place-details">
      <form method="dialog">
        <h1>Name</h1>
        <p id="Place-id">name:</p>
        <input type="text" v-model="currentPlace.name" />
        <h1>Address</h1>
        <p id="Place-id">address:</p>
        <input type="text" v-model="currentPlace.address" />
        <button @click="placesUpdate(currentPlace)">Update</button>
        <button @click="placesDestroy(currentPlace)">Delete</button>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>

<template>
  <div class="home">
    <!-- <h1>{{ message }}</h1>
    <h2>Wow amazing</h2>
    <p>{{ message2 }}</p> -->
    <h1>New place</h1>
    <div>
      <p></p>
      Name:
      <input type="text" v-model="newplaceParams.name" />
      <p></p>
      Address:
      <input type="text" v-model="newplaceParams.address" />
      <p></p>
    </div>
    <button v-on:click="createplace()">Create</button>
    <div v-for="place in places" :key="place.id">
      <h1>{{ place.name }}</h1>
      <h2>{{ place.address }}</h2>
      <img v-bind:src="place.image_url" alt="place.name" />
      <p></p>
      <button v-on:click="showplace(place)">More info!</button>
      <button v-on:click="updateplace(place)">Update!</button>
      <button v-on:click="destroyplace(place)">Destroy!</button>
    </div>
    <dialog id="place-details">
      <form method="dialog">
        <h1>place Info!</h1>
        <p>
          Name:
          <input type="text" v-model="currentplace.name" />
        </p>
        <p>
          Address:
          <input type="text" v-model="currentplace.address" />
        </p>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>
<style></style>
<script>
import axios from "axios";

export default {
  data: function () {
    return {
      // message: "Welcome to The Best Store!",
      // message2: "Teeny owl feet",
      places: [],
      newplaceParams: {},
      currentplace: {},
    };
  },
  created: function () {
    this.indexplaces();
  },
  methods: {
    indexplaces: function () {
      axios.get("http://localhost:3000/places").then((response) => {
        this.places = response.data;
        console.log("All places:", this.places);
      });
    },
    createplace: function () {
      console.log("Making a place");
      axios
        .post("http://localhost:3000/places", this.newplaceParams)
        .then((response) => {
          console.log("Nice", response.data);
          this.places.push(response.data);
        })
        .catch((error) => console.log(error.response));
    },
    showplace: function (place) {
      console.log(place);
      this.currentplace = place;
      document.querySelector("#place-details").showModal();
    },
    updateplace: function (place) {
      var editplaceParams = place;
      axios.patch("http://localhost:3000/places/" + place.id, editplaceParams).then((response) => {
        console.log("You did it!", response.data);
      });
    },
    destroyplace: function (place) {
      axios.delete("http://localhost:3000/places/" + place.id).then((response) => {
        console.log("Success!", response.data);
        var index = this.places.indexOf(place);
        this.places.splice(index, 1);
      });
    },
  },
};
</script>

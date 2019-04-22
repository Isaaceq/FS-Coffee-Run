<template>
  <div class="container">

      <!-- SEARCH INPUT AND SEARCH BUTTON. -->
    <div class="input-group mb-3" for="validationTooltip03">
      <input
        type="text"
        id="validationTooltip03"
        class="form-control"
        placeholder="address, neighborhood, city, state or zip"
        aria-label="Recipient's username"
        aria-describedby="button-addon2"
        v-model.lazy="inputCity"
      >
      <div class="invalid-tooltip">Please provide a valid city.</div>
      <div class="input-group-append">
        <button class="btn btn-info" type="button" id="button-addon2" @click="findCoffee">Search</button>
      </div>
    </div>

    <!-- DISPLAY DATA FROM FOURSQUARE API INTO A LIST. DATA BEING DISPLAY IS VENUE-NAME, VENUE-CATEGORIES, VENUE-ADDRESS. -->
    <div class="list-group" v-bind:key="venue.id" v-for="venue in venues">
      <a
        data-toggle="collapse"
        v-bind:href="'#A'+venue.id"
        class="list-group-item list-group-item-action"
      >
        <div class="d-flex w-100 justify-content-between drop-down">
          <h4 class="mx-auto venue-name">{{venue.name}}</h4>
        </div>
        <small class="categories">{{venue.categories[0].name}}</small>
        <small class="address">{{venue.location.formattedAddress[0]}}</small>
      </a>

      <!-- DISPLAY DROP-DOWN WITH GOOGLE MAP USING GOOGLE-MAP-EMBED API. USED FOURSQUARE API TO GET ADDRESS, LAT, LNG. -->
      <div class="collapse more-info" v-bind:id="'A'+venue.id">
        <div class="card card-body">
          <iframe
            width="100%"
            height="300px"
            frameborder="0"
            style="border:0"
            v-bind:src="'https://www.google.com/maps/embed/v1/place?key=AIzaSyAGsBG8kGf7mvhuqLwYNPgf3rdxGFfbqnY&q='+venueName(venue.location.address+'+'+ venue.location.city)+'&zoom=14&center='+venue.location.lat+','+venue.location.lng+''"
          ></iframe>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// USED AXIOS-JS TO FETCH DATA FROM FOURSQUARE API
import axios from "axios";

let userCity = "";

// FOURSQUARE CLIENT-ID AND CLIENT-SECRET
const clientID = "YTKSKP2I4AEXA4TOK4RKT34ZE1G0LSZ0VUDGI05VRK44EQ3K";
const clientSecret = "FEYQR1KZHJE3I4BYTQOZKT4W2J1DKSCTAPU41D1DD2KMTLCQ";

// SET CURRENT DATE AS VERSION PARAMETER
const today = new Date();
let versioning =
  today.getFullYear() + "0" + (today.getMonth() + 1) + "" + today.getDate();

// API URL AND MY USERLESS AUTH
const url = "https://api.foursquare.com/v2/";
const auth = `client_id=${clientID}&client_secret=${clientSecret}&v=${versioning}`;

export default {
  data() {
    return {
      inputCity: "",
      venues: [],
      venuesDetails: [],
      placeName: ""
    };
  },

  methods: {
    // SETS INPUT VALUE TO userCity VARIABLE
    findCoffee() {
      if (this.inputCity != "") {
        userCity = this.inputCity;
      }
    },
    // RETRIEVE VENUE ADDRESS AND REPLACE WHITE SPACES WITH + TO USE ON GOOGLE MAPS
    venueName(userStr) {
      if (this.placeName != userStr) {
        name = this.placeName;
        userStr = userStr.replace(/\s+/g, "+");
        name = userStr;
        return name;
      } else {
        this.placeName = "";
      }
    }
  },
// SET HOOKS TO HANDLE DATA FROM SQUARE API
  beforeUpdate() {
    if (this.inputCity != "") {
      this.findCoffee();
    }
  },
// GET DATA FROM THE LOCATION THAT WAS INPUTTED
  updated() {
    if (this.inputCity != "") {
      axios
        .get(
          `${url}venues/search?near=${userCity}&query=coffee&limit=15&${auth}`
        )
        .then(res => (this.venues = res.data.response.venues))
        .catch(err => console.log(err))
        .then(res => (this.inputCity = ""));
    }
    this.inputCity = "";
  }
};
</script >

// CSS
<style scoped>
.more-info {
  color: #00113d;
  background-color: lightblue;
}
small {
  display: block; 
}
.categories {
  margin-bottom: 5px;
}

.venue-name {
  padding-bottom: 10px;
}

</style>


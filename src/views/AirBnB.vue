<template>
  <div class="airbnb">
    <b-container>
      <b-jumbotron header="AirBnB" lead="AirBnB Listings Search via REST APIs">
        <p>For more information visit our website</p>
        <b-btn variant="primary" href="https://bootstrap-vue.js.org/">More Info</b-btn>
      </b-jumbotron>

      <b-form-group horizontal description="Search listing by city" label="Enter city name">
        <b-form-input v-model.trim="city" @keyup.enter="search"></b-form-input>
      </b-form-group>
      <div class="d-flex justify-content-center mb-3">
        <b-spinner v-show="searching" variant="primary" label="Loading..."></b-spinner>
      </div>
      <div v-show="hasResult">
        <b-table striped hover :items="listings.data" :fields="fields"></b-table>
      </div>
    </b-container>
  </div>
</template>

<script>
import axios from "axios";

const baseUrl = "https://airbnbapidemo.azurewebsites.net/api/airbnb/listings";

export default {
  name: "AirBnB",

  data() {
    return {
      city: "New York",
      showAlert: false,
      fields: [ 
          { key: '_id', sortable: true },
          { key: 'name', sortable: true },
          { key: 'property_type', sortable: true },
       ],
      listings: [],
      hasResult: false,
      searching: false
    };
  },

  //   async mounted(){
  //     console.log("mounted");
  //     const url = baseUrl;
  //     const listings = await axios.get(url);
  //     console.log(listings);

  //   },

  methods: {
    async search() {
      this.searching = true;
      console.log("search");
      const url = `${baseUrl}/city/${this.city}`;
      console.log(url);
      this.listings = await axios.get(url);
      if (this.listings.data.length > 0) {
        this.hasResult = true;
      } else {
        this.hasResult = false;
      }
      this.searching = false;
    }
  },

  components: {}
};
</script>
